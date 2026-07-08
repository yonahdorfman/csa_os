---
name: Janitor
version: 3.0
last_updated: 2026-04-22
category: sub-skill
requires_mcp: [Notion]
optional_mcp: [Slack, BigQuery, Gmail, Google Calendar, Google Drive]
inputs:
  - ACCOUNTS_PATH: "Base path to accounts"
  - LEDGER_PATH: "Base path to Ledger"
  - NOTION_PAGE_REGISTRY: "Path to registry"
  - CSM_TIMEZONE: "IANA timezone"
output: Hygiene report saved to Ledger/dashboards/
invoke:
  - Scheduled: weekdays at 7:30am (before morning cycle)
  - "/janitor"
---

# Janitor

Context freshness, source validation, structural hygiene. Runs before the
morning cycle so the briefing can surface warnings.

## Jobs

### Job 1: Context Freshness Audit

For each account folder in `{ACCOUNTS_PATH}`:

1. Read `_context.md` header → extract `Last Updated` date
2. Classify freshness:
   - Updated within 7 days → **fresh**
   - Updated 7-14 days ago → **stale**
   - Updated >14 days ago or missing → **critical**
3. Check `_MANIFEST.md` Sources table → for each linked source, note when
   it was last fetched

Output:
```
### Job 1: Context Freshness

✅ Fresh (≤7d): {N} accounts
⚠️ Stale (7-14d): {account names}
🔴 Critical (>14d): {account names}
```

### Job 2: Source Validation

For each account's `_MANIFEST.md` Sources table, verify the source still exists:

- **Slack channel:** Call `slack_read_channel` with limit 1. If error → flag.
- **Notion page:** Fetch the Context child page by ID from registry. If error → flag.
- **SFDC link:** Just check the URL is well-formed (can't test SFDC access).
- **GDoc link:** Check if Drive MCP can find the doc (if Drive connected).
- **BQ:** Check if the account exists in `account_plans_current` (if BQ connected).

Output:
```
### Job 2: Source Validation

✅ All sources accessible: {N} accounts
⚠️ Access issues:
• {Account}: Slack channel archived
• {Account}: Notion page not found (registry stale?)
```

### Job 3: Inbox Processing

Check `{ACCOUNTS_PATH}/../Inbox/` for unprocessed items:
- Count files not in `.processed/`
- For each, try to match to an account by filename or content
- Report what's pending

Output:
```
### Job 3: Inbox

Pending: {N} items
• {filename} — likely {account} (by filename)
• {filename} — unmatched
```

### Job 4: Structural Hygiene + Drift Check

Verify the folder tree matches expectations:
- Every account folder has `_context.md` and `_MANIFEST.md`
- Every account in the local tree has a Notion registry entry
- Every registry entry points to a real Notion page (spot-check 2-3)
- `cadence.md` exists and is parseable
- `overrides.md` has no unfilled `{PLACEHOLDER}` values

**Run `reconcile check`** to detect local/Notion drift:
- Compare timestamps and content for each account
- Flag accounts where local and Notion are out of sync
- Include drift findings in the report

Output:
```
### Job 4: Structure + Sync

✅ {N}/{N} accounts have complete file sets
⚠️ Missing registry entry: {account names}
⚠️ Orphaned folders: {folder names not in registry}

Drift check:
✅ {N} accounts in sync
⚠️ {N} drifted — run `/reconcile fix` to resolve
```

### Job 5: Notion Health

- Fetch the Accounts DB → confirm it responds
- Spot-check 2 account Context pages → confirm they render
- Check registry for stale entries (page IDs that no longer resolve)

Output:
```
### Job 5: Notion

✅ Accounts DB accessible
✅ {N}/{N} spot-checked pages OK
⚠️ Stale registry entries: {account names}
```

### Job 6: Upcoming Renewals + Risk Flags

Read all `_context.md` Identity sections:
- Flag accounts with renewal date <60 days from now
- Flag accounts with health = critical or churn risk = high
- Flag open items overdue by >7 days

Output:
```
### Job 6: Alerts

🔴 Renewal <60d: {account} ({date})
⚠️ Critical health: {account}
⚠️ Overdue items: {account} — {item} (due {date})
```

## Full Report

Combine all 6 jobs into `{LEDGER_PATH}/dashboards/hygiene-report-{YYYY-MM-DD}.md`.

Log to changelog:
```
## {HH:MM} — janitor:completed

- **Type:** janitor:{ok|warning}
- **Detail:** {N} warnings. Stale: {N}. Renewals <60d: {N}. Overdue items: {N}.
```

If warnings found → include them in the morning briefing's First Things First section.

**Post to Slack (only if warnings found) — execute these calls:**

1. Read `resources/skills/personal/overrides.md` for DISPATCH_TARGET and TELEMETRY_CHANNEL

**Personal DM:**
2. Call `slack_send_message` with channel={DISPATCH_TARGET}, message:
```
🧹 Hygiene — {N} warnings: {summary of worst issue}
```
3. Record the returned message `ts`
4. Call `slack_send_message` with channel={DISPATCH_TARGET}, thread_ts={ts}, message:
```
Freshness: {N} stale, {N} critical
Sources: {N} broken ({account}: {source})
Drift: {N} accounts out of sync
Renewals <60d: {accounts}
Overdue items: {N}

Full report: Ledger/dashboards/hygiene-report-{date}.md
```

**Activity channel (structured event):**
5. If TELEMETRY_CHANNEL is set, call `slack_send_message` with channel=TELEMETRY_CHANNEL, message: a ```json code block containing:
```json
{"event":"csa_os:janitor","properties":{"distinct_id":"{CSM_EMAIL}","time":{unix_ts},"csm_name":"{CSM_NAME}","warnings":{N},"stale":{N},"broken_sources":{N},"drift":{N},"renewals_60d":{N},"overdue":{N},"version":"4.1.1"}}
```

If no warnings → no Slack post. Silence means clean.

## Notes

- Janitor is read-only. It never modifies account context or Notion pages.
- It surfaces problems; the human or other skills fix them.
- Run time: ~2 minutes for a 10-account book. Scales linearly.
- If an MCP is unavailable, skip that source's validation — don't block the whole job.
