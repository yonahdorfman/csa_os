---
name: Harvest BigQuery
version: 3.0
last_updated: 2026-04-22
category: sub-skill
requires_mcp: [BigQuery]
inputs:
  - CSM_NAME: "Full name as it appears in BQ (e.g., 'Emmett Faricy')"
  - ACCOUNTS_PATH: "Base path to accounts"
output: Account roster + per-account context for seeding
invoke:
  - Called by setup for account discovery
  - Called by assistant for sales intelligence refresh
  - "/harvest-bq"
---

# Harvest BigQuery

Primary account discovery and sales intelligence source.

## Data Model (Verified April 2026)

**`sales_intelligence.account_metadata`** — roster + Slack channels
- `csa` — CSA name (**match on this** to find accounts)
- `slack_channel_id_c` — Slack channel ID (may be null)
- `account_name`, `arr`, `industry`, `segment`, `region`
- `account_owner`, `account_owner_email`, `sales_manager`
- `health_score_vhs`, `health_score_adoption`
- `sfdc_account_id`, `website`

**`sales_intelligence.account_plans_current`** — rich context
- `executive_summary` — 1-2 paragraph summary
- `contacts_json` — JSON array: `[{name, email, interaction_count}]`
- `action_plan_json` — JSON array: `[{task, owner, timeline, status, related_docs}]`
- `risk_identified_risks_json` — JSON array of risk strings
- `strategic_goals_json` — JSON array: `[{type, description, source, mixpanel_alignment}]`
- `health_overall_assessment`, `health_usage_summary`
- `tldr_health_rating` (High/Medium/Low), `risk_renewal_level`, `sentiment_label`
- `arr`, `commercials_contract_end`, `commercials_auto_renewal`
- `company_overview_business_model`, `company_overview_news`, `company_overview_org_structure`
- `customer_architecture_tech_stack`, `customer_architecture_integrations`
- `gdoc_link` — Google Doc with full account plan
- `latest_opp_name`, `latest_opp_stage`, `latest_opp_bookings_arr`
- `full_markdown_report` — complete formatted report (reference only)

## Discovery Query (Verified)

```sql
SELECT
  m.account_name, m.csa, m.slack_channel_id_c,
  m.arr AS metadata_arr, m.industry, m.segment, m.region,
  m.account_owner, m.account_owner_email, m.sales_manager,
  m.health_score_vhs, m.health_score_adoption,
  m.sfdc_account_id, m.website,
  p.arr AS plans_arr, p.commercials_contract_end,
  p.tldr_health_rating, p.risk_renewal_level,
  p.sentiment_label, p.executive_summary,
  p.contacts_json, p.action_plan_json,
  p.strategic_goals_json, p.risk_identified_risks_json,
  p.health_overall_assessment, p.gdoc_link,
  p.latest_opp_name, p.latest_opp_stage, p.latest_opp_bookings_arr
FROM `mixpanel-sa.sales_intelligence.account_metadata` m
LEFT JOIN `mixpanel-sa.sales_intelligence.account_plans_current` p
  ON m.account_name = p.account_name
WHERE LOWER(m.csa) LIKE '%{csm_name_lower}%'
ORDER BY COALESCE(p.arr, m.arr) DESC
```

## Output Format

For each account, produce 3 separate content blocks (one per child page):

### 1. Context content (_context.md)

See templates/_context-template.md for the full format. Key sections to fill:
- **Identity** from `account_metadata`
- **Key Contacts** from `contacts_json` (filter nulls, dedupe, sort by interactions)
- **Current Health** from `health_overall_assessment` + VHS + adoption + sentiment
- **Executive Summary** from `executive_summary` (verbatim — it's well-written)
- **Active Risks** from `risk_identified_risks_json` (date each as today, status ACTIVE)
- **Open Items** from `action_plan_json` (checklist format with owner/due/status)
- **Recent History** from dates/interactions in `executive_summary`
- **Sales Intelligence** from health scores + opp data

**Contract date fields — do not conflate these:**

`commercials_contract_end` and `latest_opp` close date answer different questions and must
be written to separate, explicitly-labeled fields. Collapsing them into one ambiguous
"Renewal" field hides lapsed contracts behind a healthy-looking future date.

- **`Contract End`** = `p.commercials_contract_end` — when the CURRENT signed contract
  actually expires. This is the only field that determines whether an account is lapsed.
- **`Next Opp Target`** = the close date implied by `latest_opp_name` / `latest_opp_stage`
  (e.g. "FY28 Renewal" closing 2027-06-01) — when the NEXT contract is projected to be
  signed. This is a pipeline forecast, not a commitment, and stays unfilled/TBD until an
  opportunity exists.
- Write both to the Identity table. Never let `Next Opp Target` overwrite or stand in for
  `Contract End`.
- **Lapsed check:** if `Contract End` is in the past AND the related opportunity's stage is
  not Closed Won, flag it explicitly — e.g. `Contract End: 2026-05-31 ⚠️ LAPSED 37d, renewal
  still Stage 1: Pipeline` — so the risk is visible in Identity itself, not just in a
  separate risk log.

### 3. Manifest content (_MANIFEST.md)

```
# {account_name} — Manifest

## Sources
| Source | Type | Identifier | Status |
|--------|------|-----------|--------|
| Slack | channel | #{name} ({id}) | Active |
| BigQuery | sales_intelligence | account_plans_current | Active |
| SFDC | account | [{id}](url) | Linked |
| GDoc | account_plan | [Link]({gdoc_link}) | Active |

## Collaborators
| Name | Role | Email |
|------|------|-------|
| {CSM} | CSA | {email} |
| {AE} | AE | {ae_email} |
```

## Refresh Mode (Daily)

When called during normal daemon operation:
- Only update **Sales Intelligence** section of _context.md (safe to overwrite)
- Update Identity fields if ARR/AE/`Contract End`/`Next Opp Target` changed — re-run the
  lapsed check (above) on every refresh, since a contract that was current yesterday can
  lapse today purely from time passing, with no new BQ data at all
- Add NEW contacts from `contacts_json` not already in Key Contacts
- Add NEW action items from `action_plan_json` not already in Open Items
- **Never overwrite** Recent History, Active Risks, Open Items (human-maintained)

## Graceful Degradation

If BigQuery MCP unavailable:
```
## BQ Harvest — {date}
**Status:** BigQuery MCP unavailable. Skipping.
```
Accounts with no `account_plans_current` row get seeded from metadata only.
