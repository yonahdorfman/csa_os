---
name: Monthly Consolidation
version: 2.1
last_updated: 2026-05-01
category: cadence
inputs:
  - ACCOUNTS_PATH, LEDGER_PATH, CSM_NAME, CSM_EMAIL, CSM_TIMEZONE
  - NOTION_PAGE_REGISTRY
invoke:
  - Scheduled: 1st Monday at configured time (via orchestrator)
  - "/monthly"
---

# Monthly Consolidation

Book-level health review. Produces a report for 1:1s, QBRs, and planning.

## Section Ownership (ENFORCED)

This skill generates a REPORT — it does NOT rewrite `_context.md`.
Human-owned sections: append-only. `[x]`/`[~]` immutable.
Machine-owned (Sales Intelligence): safe to overwrite.

## Steps

### 1. Pull Latest from Notion
### 2. Read Data

- Read changelog files for the entire previous month
- Read all account `_context.md` files (including V/R/G Log)

### 3. Generate Report

```markdown
## Monthly Review — {month} {year}

### Book Overview
- Accounts: {N} | ARR: ${total}
- Healthy: {N} | At risk: {N} | Critical: {N}
- Renewals next 90d: {N} (${arr at risk})

### Account Health
| Account | Health | ARR | Renewal | This Month |
|---------|--------|-----|---------|------------|
| {name} | {status} | ${arr} | {date} | {one-liner} |

### Risk Register
- Active: {N} | New: {N} | Resolved: {N} | Escalated: {N}

### Value Delivered
(Sourced from V/R/G Log VALUE entries)

### Goal Progress
(Sourced from V/R/G Log GOAL entries)

### Stale
- >21 days no activity: {accounts}

### Next Month
1. {priority}
2. {priority}
3. {priority}
```

### 4. Save to Ledger and Notion

1. Save to `{LEDGER_PATH}/monthly/{YYYY-MM}.md`
2. Push to Notion Workspace Ledger as child page
3. Archive changelog files >90 days to `{LEDGER_PATH}/archive/`

### 5. Post to Slack (headline + thread)

1. Read `resources/skills/personal/overrides.md` for DISPATCH_TARGET
2. Call `slack_send_message` with channel={DISPATCH_TARGET}, message:
```
📊 Monthly — {month} — ${arr} ARR, {N} accounts, {N} at-risk, {N} renewals next 90d
```
3. Record `ts`
4. Call `slack_send_message` with channel={DISPATCH_TARGET}, thread_ts={ts}, message:
```
Book: ${arr} | {N} healthy, {N} at-risk, {N} critical

Highlights:
• {Account} — {biggest change}

Risks:
• {Account} — {top risk}

Stale (>21d): {accounts or "none"}

Next month:
1. {priority}
2. {priority}

Full report: Ledger/monthly/{YYYY-MM}.md
```

### 6. Post to Activity Channel (structured event)

1. Read `resources/skills/personal/overrides.md` for TELEMETRY_CHANNEL
2. If TELEMETRY_CHANNEL is set, call `slack_send_message` with channel=TELEMETRY_CHANNEL, message: a ```json code block containing:
```json
{"event":"csa_os:briefing_posted","properties":{"distinct_id":"{CSM_EMAIL}","time":{unix_ts},"type":"monthly","csm_name":"{CSM_NAME}","accounts":{N},"arr":{total},"at_risk":{N},"renewals_90d":{N},"version":"4.1.1"}}
```

### 7. Log to Changelog

```
## {HH:MM} — monthly:completed
- **Dispatched:** DM + activity channel
```
