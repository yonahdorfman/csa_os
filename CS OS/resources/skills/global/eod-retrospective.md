---
name: End-of-Day Retrospective
version: 3.1
last_updated: 2026-04-30
category: cadence
inputs:
  - ACCOUNTS_PATH, LEDGER_PATH, CSM_NAME, CSM_TIMEZONE
  - NOTION_PAGE_REGISTRY
output: Context patches written to Notion + retro posted to Slack + Ledger
invoke:
  - Scheduled: weekdays at configured time (via assistant evening cycle)
  - "/retro"
---

# End-of-Day Retrospective

Reads today's changelog. Consolidates what happened. Writes final context
patches to Notion. Posts summary to Slack.

If the daemon was down and retros were missed, backfills for each skipped day.

## Key Principle

The retro reads the CHANGELOG, not the sources. The harvesters already ran.
The retro consolidates and reflects.

## Backfill Detection

Before running today's retro, check for missed days:

1. Read `{LEDGER_PATH}/retros/` — find the most recent `*-eod.md` file
2. Compare that date to today
3. If there are gaps (workdays with no retro file), backfill each one

**Example:** Today is Wednesday April 30. Last retro file is `2026-04-25-eod.md` (Friday).
Monday April 28 and Tuesday April 29 have no retros. Backfill both before running today's.

### Backfill Steps (per missed day)

For each missed workday (skip weekends):

1. Check if a changelog exists for that day (`{LEDGER_PATH}/changelog/{date}.md`)
   - If yes → use it to generate a retro (same as normal retro steps below)
   - If no → generate a minimal retro noting the daemon was down:

```markdown
## EoD Retro — {Day}, {Month} {Date} (BACKFILLED)

### Note
Daemon was offline on this date. No changelog available.
Retro generated retroactively on {today} from Notion context state.

### Account State (as of backfill)
- {Account}: {current health/status from Notion context}
```

2. Check Slack and Gmail for signals from that day (if harvesters are available)
   - Any signals found get compressed into context patches
   - This recovers signals that would have been caught if the daemon was running

3. Save the backfilled retro to `{LEDGER_PATH}/retros/{missed-date}-eod.md`
4. Push to Notion Workspace Ledger
5. Log: `eod-retro:backfilled:{date}`

### Backfill Slack Posting

Post ONE headline for all backfills combined (not one per day):

Top-level:
```
🌙 EoD — backfilled {N} missed day(s) ({date range}), plus today
```

Thread includes summaries for each backfilled day, then today's retro.

### Backfill Limits

- Maximum backfill: 5 workdays (one week). Beyond that, the monthly
  consolidation is a better tool for catching up.
- If >5 days missed, note it: "Daemon was offline for {N} days. Run
  `/monthly` to consolidate the gap."

---

## Normal Retro (today)

### 1. Pull Latest from Notion
Pull all account Context pages to local cache. Ensures we have any human
edits made during the day.

### 2. Read Today's Changelog
Read `{LEDGER_PATH}/changelog/{YYYY-MM-DD}.md`.
Extract: accounts touched, signals processed, context updates, errors.

### 3. Read Account Context
For each account touched today, read current `_context.md`.

### 4. Prompt for Human Input (Optional)
If running interactively:
- "Anything to add from calls or conversations today?"
- "Any risks to flag that the daemon missed?"

If running on schedule (no human present), skip.

### 5. Write Context Patches to Notion

**Section ownership enforced.** All sections below are human-owned.
Append only. Never overwrite existing entries. `[x]` and `[~]` are immutable.

For accounts touched today, write to the Notion Context page:
- **Recent History:** Append "EoD review" entries for anything compress missed
- **Open Items:** Mark resolved items as `[x]`, mark overdue items with ⚠️. Never remove or re-open `[x]`/`[~]` items.
- **Active Risk:** Append status updates to existing risks. Never remove closed risks.
- **V/R/G Log:** Append VALUE, RISK, or GOAL entries from today
- **Sales Intelligence:** Safe to overwrite (machine-owned zone)

Deduplicate: match by date + first 50 chars. If it's already there, skip.

Write to Notion first. Cache locally after.

### 6. Generate Retro Content

**Full retro (saved to Ledger + pushed to Notion Workspace Ledger):**

```markdown
## EoD Retro — {Day}, {Month} {Date}

### Done
- {Account}: {what was accomplished or happened}
- {Account}: {what was accomplished}

### Carrying
- {Account}: {what didn't get done and why}

### Signals
- Sources: Gmail {N} | Slack {N} | Calendar {N} | BQ {N} | Mixpanel {N}
- Actionable: {N} | Noise filtered: {N}
- Context updates: {N} accounts patched

### Tomorrow
1. {Top priority — specific account + action}
2. {Second priority}
3. {Third priority}
```

### 7. Save to Ledger and Notion

1. Save to `{LEDGER_PATH}/retros/{YYYY-MM-DD}-eod.md`
2. Push retro to Notion Workspace Ledger as child page

### 8. Post to Slack (headline + thread)

Execute these `slack_send_message` calls — do not skip.

1. Read `resources/skills/personal/overrides.md` for DISPATCH_TARGET and TELEMETRY_CHANNEL

**Post to personal DM:**
2. Call `slack_send_message` with channel={DISPATCH_TARGET}, message:
```
🌙 EoD — {N} accounts touched, {N} carrying, tomorrow: {top priority}
```
3. Record the returned message `ts`
4. Call `slack_send_message` with channel={DISPATCH_TARGET}, thread_ts={ts}, message:
```
Done:
• {Account} — {what happened}

Carrying:
• {Account} — {what didn't get done}

Tomorrow:
1. {top priority}
2. {second}
3. {third}

Signals: {N} processed | Context: {N} accounts patched
Full retro: Ledger/retros/{date}-eod.md
```

### 9. Post to Activity Channel (structured event)

1. If TELEMETRY_CHANNEL is set, call `slack_send_message` with channel=TELEMETRY_CHANNEL, message: a ```json code block containing:
```json
{"event":"csa_os:briefing_posted","properties":{"distinct_id":"{CSM_EMAIL}","time":{unix_ts},"type":"eod","csm_name":"{CSM_NAME}","accounts_touched":{N},"carrying":{N},"patches":{N},"version":"4.1.1"}}
```
2. If it fails, log and continue.

### 10. Log to Changelog

```
## {HH:MM} — eod-retro:completed
- **Dispatched:** DM + activity channel
```
