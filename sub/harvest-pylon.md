---
name: Harvest Pylon
version: 1.0
last_updated: 2026-07-01
category: sub-skill
requires_mcp: [Pylon]
inputs:
  - CSM_NAME: "Your name as it appears in account ownership fields"
  - ACCOUNTS_PATH: "Base path to accounts"
  - LOOKBACK_DAYS: "default: 30"
output: Support ticket signals per account
invoke:
  - Called by harvest-all during daemon cycle
  - "/harvest-pylon"
---

# Harvest Pylon

**Pylon is the only support-ticket source called by harvest-all as of
2026-07-16** — Mixpanel migrated off Zendesk to Pylon, and `harvest-zendesk.md`
has been moved to standby (see that file and harvest-all.md's "Not called by
default" section). If pre-migration ticket history is ever needed, reactivate
Zendesk manually rather than relying on it in the regular cycle.

Pull open and recent support tickets ("issues" in Pylon's model) for your
accounts directly via the Pylon MCP — there is no BigQuery table for Pylon
yet (confirmed via a live `bq list-tables` check on 2026-07-16), so this
harvester is MCP-only for now. **Per harvest-all.md's BQ-first policy, the
moment a `pylon`-equivalent BQ table exists, switch this harvester to query
it first and drop to the Pylon MCP only for same-day tickets not yet
ingested** — same pattern as `harvest-gong.md`. Until then, MCP-only is
correct, not a gap to keep flagging. Ticket patterns are early churn signals —
volume spikes, repeated issues, high-severity tickets, and negative sentiment
all indicate product friction.

## Tools (Pylon MCP)

| Tool | Use |
|------|-----|
| `search_accounts` | Resolve an account name to a Pylon account/customer ID |
| `search_issues` | Find issues, filterable by account, status, date range |
| `get_issue` | Full detail on a specific issue |
| `get_issue_messages` | Thread/message history for an issue |
| `get_account` | Account-level metadata (contacts, plan, etc.) |
| `get_contact` | Contact detail for a reporter |

Exact parameter names may differ slightly from the table above — confirm
against the live tool schema the first time you run this (`ToolSearch` for
`pylon` if the tools aren't already loaded), then this note can be deleted.

## Per-Account Flow

For each account in `ACCOUNTS_PATH`:

1. `search_accounts` with the account/company name to resolve a Pylon
   account ID (cache this in `_context.md` front matter once found, so
   future runs skip the lookup).
2. `search_issues` filtered by that account ID (or by account name if no
   ID filter is available) and `created_at >= now - LOOKBACK_DAYS`.
3. For any issue flagged high-severity, escalated, or with no reply in
   >24h, call `get_issue` (and `get_issue_messages` if the summary isn't
   enough) for full context.

If `search_accounts` returns no match, fall back to a free-text
`search_issues` query using the account name and note in the output that
the account isn't yet mapped in Pylon (may mean zero tickets, or a naming
mismatch worth investigating).

## Signal Classification

| Condition | Signal | Urgency |
|-----------|--------|---------|
| Priority/severity = Urgent or High | 🔴 High-severity ticket | critical/high |
| Status = open + no agent reply in >24h | Customer waiting, unanswered | high |
| Sentiment reads negative in messages | Customer frustrated | high |
| 3+ issues from same account in lookback | Volume spike — possible product friction | high |
| Issue tagged bug/defect with no workaround | No workaround — customer blocked | critical |
| Issue tagged feature request | Feature gap signal | normal |
| Recurring root cause across issues | Confirms existing risk | normal |

## Patterns to Detect

Beyond individual tickets, look for:
- **Volume trend:** Is this account filing more tickets than usual?
- **Product concentration:** Are tickets clustering around one feature?
- **Repeat issues:** Same root cause or topic appearing in multiple tickets?
- **Response gaps:** Tickets sitting without an agent reply?

These patterns map to V/R/G Log RISK entries.

## Output Format

```markdown
## Pylon Harvest — {date}

**Tickets found:** {N} across {N} accounts (last {LOOKBACK_DAYS} days)

### {Account Name} — {N} tickets
**Trend:** {stable | increasing | new}

#### [{issue_title}]({issue_url})
- **Status:** {status} | **Priority:** {priority} | **Sentiment:** {sentiment if available}
- **Summary:** {summary of issue/first message}
- **Reporter:** {contact name}
- **Signal:** {risk classification}

### Unmapped Accounts
{accounts where search_accounts found no Pylon match — flag for investigation}

### No Tickets
{accounts with zero tickets in the lookback window}
```

## Integration with Compress

Pylon harvest output feeds into compress like any other harvester.
Compress will:
- Add ticket signals to Active Risk or V/R/G Log
- Flag volume spikes and unanswered tickets as high urgency
- Note account-level ticket trend in Executive Summary where relevant

## Historical Tickets (Pre-Migration)

Tickets filed **before the Zendesk → Pylon cutover** live only in BQ (Zendesk
tables) and are not in Pylon. `harvest-zendesk.md` is on standby, not run by
default — if a specific account needs pre-migration ticket history, run it
manually (`/harvest-zendesk`) rather than expecting it in the regular cycle.

## Notes

- Zero tickets is a positive signal — include "No open tickets in Pylon" in
  the Sales Intelligence section to confirm it was checked, not skipped.
- Ticket volume spikes (3+ in a week from one account) should trigger an
  immediate alert via Slack dispatch, not wait for the next briefing.
- Cache the resolved Pylon account ID per account (e.g. in `_context.md`
  front matter or a lookup table) once found, to avoid re-resolving on every
  run — `search_accounts` by name is the only per-run cost that scales with
  account count.
