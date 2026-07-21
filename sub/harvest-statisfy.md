---
name: Harvest Statisfy
version: 1.0
last_updated: 2026-07-21
category: sub-skill
requires_mcp: [Statisfy]
inputs:
  - ACCOUNTS_PATH: "Base path to accounts"
  - CSM_NAME: "Your name"
output: Live CRM enrichment per account (health scorecard, pipeline, contacts, tasks)
invoke:
  - Called by harvest-all during daemon cycle
  - "/harvest-statisfy"
---

# Harvest Statisfy

**Second-source enrichment layer, not a replacement for BQ.** `sales_intelligence.account_plans_current`
(harvest-bq.md) is the CS-authored account plan — executive summary, action plan, risks — and it can
lag. Statisfy is the live CRM mirror: current opportunity stage/amount/probability straight from
Salesforce, the 6-pillar health scorecard, the CSM/AM assignment of record, and the raw contact/task
rows underneath the narrative. Pull both every cycle; never treat one as authoritative over the other
without cross-checking — see "Conflicts with BigQuery" below.

Statisfy tools are read-only, so this can run unattended across the full book with no write-conflict risk.

## Step 0 — Resolve account_name → Statisfy cust_id (cache per account, resolve once)

Statisfy's account name search is fuzzy and noisy — one company name can return 5-10 rows (regional
entities, deleted stubs, lookalike domains), and there is no direct filter on billing account ID
(`list_objects` errors if you try). Resolve via:

1. `search_objects(object_type='account', query='<account name>')` → candidate rows with `id`, `name`,
   `domain`, `type`, `arr_dollar`, `parent_id`.
2. Prefer candidates where `type='Customer'` and `arr_dollar > 0` as a first guess — but do NOT trust
   name/ARR alone. Confirm with step 3.
3. `get_objects(object_type='account', ids=[<candidate id>])` → read the custom field
   `billing_account_id_for_finance__c__e_salesforce_`. If it matches the account's known Billing
   Account ID from the roster (BQ `account_metadata`), this is the account. If no candidate matches,
   widen the `search_objects` query (try domain, try parent company name) before marking the account
   "no match."
4. **Never guess a cust_id match below full confidence on the billing ID field** — a wrong match
   silently pollutes the account file with another customer's health/pipeline data.
5. Cache `{account_name: {cust_id, billing_account_id, crm_account_id}}` for the whole run. Every other
   call in this skill filters on `cust_id` — resolve once per account per run, reuse everywhere. Only
   re-resolve if the account name or billing ID changed since the cache was built.

The full `get_objects(account, ...)` response from step 3 is worth keeping in full, not just the
billing-ID field — it's the richest single row available (renewal date, contract term, tier, plan cap,
utilization rates, AM-written health/relationship narrative, Slack channel IDs, GCP likelihood, etc.).
Treat it as a second Identity source alongside BQ metadata.

**Performance:** batch step 0 for every account in the run first and build the full cust_id map before
starting the per-account pulls below — this avoids re-searching the same fuzzy name repeatedly.

## Per-Account Pulls (all scoped by the resolved cust_id)

### 1. Health Scorecard
`list_objects(object_type='account_health', filters={'cust_id': <cust_id>, 'is_latest': true})`

One row per pillar: `productUsage`, `relationship`, `outcomes`, `experience`, `featureRequests`, `risks`,
`overallHealth`. Each carries a label (e.g. "Very Negative", "Slightly Negative", "Neutral", "Positive")
and a date. This is more granular than BQ's single `tldr_health_rating` and is the fastest way to spot
which specific dimension is dragging an account down — `risks` and `experience` both "Very Negative"
while `relationship` is only "Slightly Negative" tells a different story than a flat "at risk."

### 2. Open Opportunities
`list_objects(object_type='opportunity', filters={'cust_id': <cust_id>, 'is_closed': false})`

Then `get_objects(object_type='opportunity', ids=[...])` for any deal worth surfacing in detail. Full
rows include `amount`, `stage`, `type` (Renewal/Upsell/New Business), `close_date`, `owner_id`, and a
nested `crm_opportunity` object with the live Salesforce record — `Probability`, `ForecastCategory`,
`Renewal_at_Risk__c`, `Sales_Play__c`, `Next_Step__c`, `Churn_Likelihood__c`. This is renewal/expansion
pipeline ground truth — cross-check against BQ's `latest_opp_name` / `latest_opp_stage` (BQ may be a
stale snapshot) and prefer the Statisfy stage/close_date where they diverge.

**Lapsed-contract check applies here too:** if an opportunity's `type` is Renewal and its `close_date`
has passed with `is_closed=false`, that's the same lapsed-contract signal harvest-bq.md flags on
Contract End — surface it explicitly, the same way, not buried in a risk list.

### 3. Relationship of Record (CSM/AM assignment)
`get_relationships(relation='account_people', filters={'cust_id': <cust_id>})`

Role-tagged rows (`type`: CSM, AM, Champion, Executive Sponsor, etc.) with either a `people_id`
(contact-side) or `user_id` (internal Mixpanel-side). This is the system-of-record assignment — use it
to confirm the CSA/AM pairing in Identity matches CRM, and to catch stale ownership after a book handoff.

### 4. Key Contacts
`list_objects(object_type='contact', filters={'cust_id': <cust_id>}, limit=50)`

Name/email/role/title/department. Merge into the existing Key Contacts table sourced from BQ's
`contacts_json` — dedupe on email, keep BQ's `interaction_count` where present, and add any contact
Statisfy has that BQ doesn't (Statisfy pulls from live CRM contact sync, so it sometimes has people BQ's
snapshot missed).

### 5. Open Tasks (CRM-side, distinct from BQ's action_plan_json)
`list_objects(object_type='task', filters={'cust_id': <cust_id>, 'status': ['TODO','IN_PROGRESS']})`

Salesforce/CS-platform tasks, not the BQ-authored account plan. Add as a supplementary checklist —
label the source ("CRM Task" vs "Account Plan") so the two lists aren't confused, since they can
diverge on owner and due date.

### 6. Optional — Deep Signal Check (on-demand only, not part of the bulk run)
`semantic_search(scope='transcripts', query='<topic>', customer_ids=[<cust_id>], date_range_days=90)`

Use for spot-checks on a specific topic (e.g. before a QBR, or to corroborate a risk BQ flagged against
actual call/email content). **Skip this in bulk full-book runs** — it's the expensive/targeted tool, not
the bulk one. Only run it when a session explicitly asks for a deeper pass on one account.

**Tool limits:** `list_objects` / `get_relationships` default to `limit=10` — raise to 50-100 for
contacts and tasks on accounts with heavy CRM activity, or results silently truncate.

## Output Format — append to existing `_context.md`

Add a new `## Statisfy Snapshot` section. **Do not merge into BQ's Sales Intelligence section** — keep
the two sources distinguishable so a reader (and compress) always knows which system said what.

```markdown
## Statisfy Snapshot
> 🤖 Machine-generated — live CRM data as of {date}. Do not edit manually; overwritten each refresh.

**Health Scorecard** (as of {health.date})

| Pillar | Status |
|--------|--------|
| Overall | {overallHealth} |
| Product Usage | {productUsage} |
| Relationship | {relationship} |
| Outcomes | {outcomes} |
| Experience | {experience} |
| Feature Requests | {featureRequests} |
| Risks | {risks} |

**Open Opportunities**

| Name | Type | Stage | Amount | Close Date | Probability | Renewal at Risk? |
|------|------|-------|--------|------------|--------------|-------------------|
| {name} | {type} | {stage} | ${amount} | {close_date} | {probability}% | {Yes/No} |

**Relationship of Record**
- CSM: {name} | AM: {name} | Champion: {name or "none logged"} | Exec Sponsor: {name or "none logged"}

**Key Contacts (Statisfy)** — merged into the main Key Contacts table above; new adds marked (NEW)

**Open CRM Tasks**
- [ ] {title} — due {due_date}

**Conflicts with BigQuery** (only if any)
- e.g. "BQ lists Contract End 2027-05-31; Statisfy CRM shows an active Renewal opp with close date
  2026-08-31 and Renewal_at_Risk=true — confirm which is current."
```

## Refresh Mode (Daily)

- **Health Scorecard, Open Opportunities, Open Tasks:** always safe to overwrite — live CRM state.
- **Relationship of Record:** overwrite, but log a note in the changelog if it changed since last run —
  ownership changes are worth surfacing, not silently absorbing.
- **Key Contacts:** additive merge only — never delete a contact BQ or a human added.
- **cust_id resolution (Step 0):** reuse the cached value; only re-run if the account name or billing ID
  changed.

## Conflicts with BigQuery

If BQ and Statisfy disagree on ARR, contract end date, or CSM/AM assignment — or on renewal
stage/close-date per the lapsed-contract check in Step 2 — do not silently pick one. Record both values
with source attribution in the `Conflicts with BigQuery` line of the output above, and let compress carry
it into the account's Active Risk or Recent History rather than resolving it unilaterally (same rule
compress.md already applies to any two harvesters that disagree — see its "Conflicting signals" edge
case).

## Graceful Degradation

If the Statisfy MCP is unavailable, or an account can't be resolved to a `cust_id` after a widened
search:

```
## Statisfy Harvest — {date}
**Status:** Statisfy unavailable / no confident account match. Skipping. (BQ data only.)
```

Statisfy is an optional enrichment source, not part of `HARVEST_REQUIRED` in orchestrator.md — a missing
Statisfy MCP or an unresolved account should never block a harvest cycle or trigger harvest-catchup.

## Integration with Compress

Statisfy harvest output feeds into compress like any other harvester. Compress will:
- Write the `## Statisfy Snapshot` section (machine-owned, safe to overwrite each refresh, same as
  Sales Intelligence).
- Merge new Statisfy contacts into the human-owned Key Contacts table additively.
- Surface Health Scorecard pillar drops, lapsed-Renewal-opp signals, and BQ/Statisfy conflicts as Active
  Risk or V/R/G Log RISK entries where they indicate something new.

## Notes

- Statisfy tools are read-only — no write-conflict risk, safe to run unattended across the full book.
- Batch Step 0 for all accounts first, cache the cust_id map, then run Steps 1-5 per account — avoids
  re-searching the same fuzzy name repeatedly.
- Zero open opportunities or zero open tasks is a positive signal — state it explicitly ("No open
  opportunities in Statisfy") rather than leaving the section blank, so a reader knows it was checked.
