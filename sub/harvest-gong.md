---
name: Harvest Gong
version: 1.0
last_updated: 2026-06-30
category: sub-skill
requires_mcp: [BigQuery]
optional_mcp: [Gong]
inputs:
  - CSM_NAME: "Your name as it appears in BQ csa column"
  - ACCOUNTS_PATH: "Base path to accounts"
  - LOOKBACK_DAYS: "default: 7"
output: Call summaries, action items, and key takeaways per account
invoke:
  - Called by harvest-all during daemon cycle
  - "/harvest-gong"
---

# Harvest Gong

Pull call transcripts, summaries, key takeaways, and action items from BQ.
This is one of the richest signal sources — a single call produces structured
action items with owners, detailed takeaways, and participant lists.

**Gong is the only call intelligence source called by harvest-all** — use
this harvester. `harvest-clari.md` is on standby (not called by default); it's
kept on disk only in case the old Clari dataset ever needs cross-checking
against Gong, and shouldn't be run without asking first.

## Data Model (Verified)

**`sales_intelligence.gong_calls`** — call recordings and analysis (partitioned by `call_timestamp`)

| Column | Description |
|--------|-------------|
| `call_id` | Unique call identifier |
| `sfdc_account_id` | Salesforce account ID |
| `account_name` | Account name (join key) |
| `csa` | CSA name (**match on this**) |
| `call_title` | Meeting title |
| `call_timestamp` | When the call happened |
| `call_duration_seconds` | Call length |
| `participants_external` | Array of external attendee emails |
| `participants_internal` | Array of internal attendee emails |
| `full_summary` | Complete call summary |
| `key_takeaways` | Bullet-pointed key insights |
| `action_items` | Structured action items with owners |
| `transcript` | Full call transcript (long) |
| `call_url` | Link to Gong recording |
| `deal_name` | Associated deal if any |
| `sfdc_opportunity_id` | Linked SFDC opportunity |
| `industry` | Account industry |
| `segment` | Account segment |
| `region` | Account region |
| `arr` | Account ARR |
| `account_owner` | Account owner (AE/AM) |
| `ingested_at` | When this row was ingested into BQ |

## Query

```sql
SELECT
  call_id, account_name, call_title, call_timestamp,
  call_duration_seconds, participants_external, participants_internal,
  full_summary, key_takeaways, action_items, call_url, deal_name
FROM `mixpanel-sa.sales_intelligence.gong_calls`
WHERE LOWER(csa) LIKE '%{csm_name_lower}%'
  AND call_timestamp >= TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL {LOOKBACK_DAYS} DAY)
ORDER BY call_timestamp DESC
```

Do NOT pull the `transcript` column in the default query — it's very long
and burns tokens. Only fetch transcripts on-demand for specific calls.

## Processing Each Call

For each call returned:

### 1. Extract Key Takeaways

The `key_takeaways` column is already bullet-formatted. Parse each bullet as
a separate takeaway. These map to Recent History entries.

### 2. Extract Action Items

The `action_items` column contains structured items with owners. Parse each one:
```
- {task description} (Owner: {PERSON_NAME})
```

Map each to an Open Item in `_context.md`:
```
- [ ] ⚠️ {task} — {owner} — from {call_title} on {date} [via Gong]
```

Check for duplicates: if the item already exists in Open Items (fuzzy match
on task description), skip it. If it exists but status changed, update it.

### 3. Extract Contact Activity

The `participants_external` array gives you customer contacts who were on the call.
Update Key Contacts section: set `last touched: {call_date}` for each participant.
If a participant isn't in Key Contacts yet, add them.

### 4. Extract Sentiment and Risk Signals

Read the `key_takeaways` and `full_summary` for risk indicators:
- Technical blockers ("data corruption", "unusable", "broken")
- Competitive mentions ("evaluating", "alternative", "Optimizely")
- Churn signals ("budget concerns", "scaling back", "frustrated")
- Positive signals ("excited", "expanding", "deeply engaged")

Tag as: risk (high urgency) or positive (normal urgency).

## Output Format

```markdown
## Gong Harvest — {date}

**Calls found:** {N} in last {LOOKBACK_DAYS} days

### {Account Name} — {call_title}
**Date:** {timestamp} | **Duration:** {minutes}m
**External:** {participant names}
**Deal:** {deal_name or "none"}

**Key Takeaways:**
{key_takeaways — verbatim from BQ}

**Action Items:**
{action_items — verbatim from BQ}

**Signals:**
- {risk|positive}: {one-line interpretation}

**Gong link:** {call_url}

---

### {Next account/call}
...
```

## Integration with Compress

Gong harvest output feeds into compress like any other harvester.
Compress will:
- Add call summary to Recent History (with source: "via Gong call")
- Add action items to Open Items (deduplicating against existing)
- Update contact last-touched dates
- Flag risk/positive signals in Active Risk or V/R/G Log

## Gong MCP (Optional — Real-Time Only)

The Gong MCP (`get_recent_calls`, `get_call_detail`, `search_calls`) provides
calls from the **current day** that haven't been ingested into BQ yet. The BQ
table is refreshed daily at ~6:30 AM PT.

Use the MCP when:
- You need today's calls (after the last BQ refresh)
- BQ appears to be missing recent days (DAG failure)

Use BQ for the scheduled harvest — it's faster, cheaper, and has no rate limits.
Max MCP lookback: 5 days (only to cover a DAG outage). For anything older, BQ only.

## Notes

- Call transcripts are large (10K+ tokens). Never pull full transcripts in the
  scheduled harvest. The summary, takeaways, and action items are sufficient.
- The `action_items` field from BQ is pre-structured with owners — this is
  the highest-value field. A single call can produce 3-5 action items that
  go directly into Open Items.
- Deduplication matters: calls often produce action items that overlap with
  existing Open Items. Match by task description, not by date.
- The table is partitioned by `call_timestamp` — the WHERE clause above
  will use partition pruning automatically.
