---
name: Harvest Zendesk
version: 1.2
last_updated: 2026-07-16
category: sub-skill
requires_mcp: [BigQuery]
status: standby — not called by default as of 2026-07-16
inputs:
  - CSM_NAME: "Your name as it appears in BQ csa column"
  - ACCOUNTS_PATH: "Base path to accounts"
  - LOOKBACK_DAYS: "default: 30"
output: Support ticket signals per account
invoke:
  - "/harvest-zendesk" (manual only)
---

# Harvest Zendesk

**Standby as of 2026-07-16.** Superseded by `harvest-pylon.md` for support
tickets and removed from `harvest-all.md`'s execution list — it is no longer
called automatically. This file is kept intact (not deleted) in case
pre-migration ticket history is needed again. To reactivate, add it back to
`harvest-all.md`'s numbered execution list — do not do so without asking
first.

**Known issue, fix before reactivating:** the query below targets
`sales_intelligence.zendesk_tickets_enriched`, which no longer exists (a live
`bq list-tables` check on 2026-07-16 confirmed it's been dropped post-migration,
matching the `table_not_found` skip logged that day). The current tables are
`sales_intelligence.zendesk_tickets_raw` and `sales_intelligence.zendesk_tickets_redacted` —
the query and column list below need to be re-verified against one of those
before this harvester can run again.

**Mixpanel migrated off Zendesk to Pylon as of July 2026. Pylon is now the
primary support ticket source — use `harvest-pylon.md` first.** Historically
this harvester surfaced **tickets filed before the migration**, which live in
BQ and were not backfilled into Pylon.

Pull support tickets from BQ for your accounts. Ticket patterns are
early churn signals — volume spikes, repeated issues, high-severity
tickets, and negative sentiment all indicate product friction.

## Data Model (Verified)

**`sales_intelligence.zendesk_tickets_enriched`** — tickets with AI analysis

| Column | Description |
|--------|-------------|
| `ticket_id` | Unique ticket ID |
| `account_name` | Account name (join key) |
| `csa` | CSA name |
| `ticket_subject` | Ticket title |
| `ticket_status` | open, pending, solved, closed |
| `ticket_created_at` | When filed |
| `ticket_url` | Link to Zendesk ticket |
| `anonymized_summary` | AI-generated summary (PII removed) |
| `products` | Mixpanel features involved |
| `use_cases` | What the customer was trying to do |
| `root_cause` | Identified root cause |
| `has_workaround` | Boolean — is there a workaround? |
| `workaround_summary` | Description of workaround if exists |
| `issue_type` | Bug, feature request, how-to, etc. |
| `severity` | Critical, High, Normal, Low |
| `sentiment` | Customer sentiment in the ticket |

## Query

```sql
SELECT
  ticket_id, account_name, ticket_subject, ticket_status,
  ticket_created_at, ticket_url, anonymized_summary,
  products, use_cases, root_cause, has_workaround,
  issue_type, severity, sentiment
FROM `mixpanel-sa.sales_intelligence.zendesk_tickets_enriched`
WHERE (LOWER(csa) LIKE '%{csm_name_lower}%'
  OR account_name IN ({account_names_list}))
  AND ticket_created_at >= TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL {LOOKBACK_DAYS} DAY)
ORDER BY ticket_created_at DESC
```

Note: The `csa` column may not be populated for all tickets. Also query
by `account_name` matching your known accounts as a fallback.

## Signal Classification

| Condition | Signal | Urgency |
|-----------|--------|---------|
| Severity = Critical | 🔴 Critical support issue | critical |
| Severity = High | ⚠️ High-severity ticket | high |
| Sentiment = negative + severity ≥ Normal | Customer frustrated | high |
| 3+ tickets from same account in lookback | Volume spike — possible product friction | high |
| `has_workaround` = false + severity ≥ High | No workaround — customer blocked | critical |
| `issue_type` = feature_request | Feature gap signal | normal |
| `root_cause` mentions known product limitation | Confirms existing risk | normal |

## Patterns to Detect

Beyond individual tickets, look for:
- **Volume trend:** Is this account filing more tickets than usual?
- **Product concentration:** Are tickets clustering around one feature?
- **Repeat issues:** Same root cause appearing in multiple tickets?
- **Sentiment trend:** Is sentiment getting worse over time?

These patterns map to V/R/G Log RISK entries.

## Output Format

```markdown
## Zendesk Harvest — {date}

**Tickets found:** {N} across {N} accounts (last {LOOKBACK_DAYS} days)

### {Account Name} — {N} tickets
**Trend:** {stable | increasing | new}

#### [{ticket_subject}]({ticket_url})
- **Status:** {status} | **Severity:** {severity} | **Sentiment:** {sentiment}
- **Summary:** {anonymized_summary}
- **Products:** {products} | **Root cause:** {root_cause}
- **Workaround:** {yes: summary | no — customer blocked}
- **Signal:** {risk classification}

### No Tickets
{accounts with zero tickets in the lookback window}
```

## Current State

**As of July 2026, Zendesk is retired for new tickets — Mixpanel moved to
Pylon.** `zendesk_tickets_enriched` stops growing at the cutover date; treat
anything in this table as historical. New support signals come from
`harvest-pylon.md`.

As of April 2026, none of Emmett's 11 accounts had tickets in
`zendesk_tickets_enriched`. The table was populated for other CSAs'
accounts (JioHotstar, Autodesk, Uber, etc.). If your accounts still show
zero rows here, that's expected — check `harvest-pylon.md` for anything
current.

## Notes

- Zero tickets is a positive signal — include "No open tickets (historical
  Zendesk)" in the Sales Intelligence section to confirm it was checked,
  not skipped.
- Only run this harvester to catch a pre-migration ticket that's still
  open or relevant to active risk. Don't expect new volume here — pair it
  with `harvest-pylon.md`, which is the primary source going forward.
- The `products` and `use_cases` fields tell you which Mixpanel features
  were causing friction historically — useful context for QBR and renewal
  prep even after the migration.
