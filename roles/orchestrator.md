---
name: Orchestrator
version: 1.0
last_updated: 2026-04-30
category: role
description: >
  THE single scheduled Cowork task (besides DM monitor). Runs hourly.
  Evaluates cadence rules, detects missed cycles, executes what's due.
  There should be NO other scheduled tasks for individual skills.
requires_mcp: [Notion, Slack]
optional_mcp: [Gmail, Google Calendar, Google Drive, BigQuery, Mixpanel MCP]
invoke:
  - Cowork scheduled task: hourly during work hours
  - "/orchestrate"
  - "/tick"
---

# Orchestrator

**This is the only scheduled skill.** (DM monitor is the only exception.)

All cadences (janitor, morning briefing, EoD retro, EoW retro, monthly)
run through this single hourly tick. Do NOT create separate scheduled tasks
for each cadence — that causes duplicated instructions, stale permissions,
and skills running outside the orchestrator's awareness.

## Cowork Scheduled Task Configuration

**Name:** CSA OS Orchestrator
**Repeats:** Hourly during work hours (7am–6pm weekdays)
**Folder:** {CSOS_PATH}
**Instructions:**

```
Read and execute the orchestrator skill at {CSOS_PATH}/roles/orchestrator.md.
Follow the skill file as written. Read cadence.md for schedule configuration.
Read the changelog to determine what has already run today. Execute whatever
is due. Write all context changes to Notion first, cache locally after.
```

**Required MCP Permissions (Always Allowed):**

| MCP | Permission | Why |
|-----|-----------|-----|
| **Notion** | Read pages, Update pages, Create pages, Search | Source of truth — read/write account context |
| **Slack** | Send message, Read channel, Read user profile, Search | Dispatch briefings, read commands, DM monitor |
| Gmail | Search threads, Get thread | Harvest email signals |
| Google Calendar | List events, Get event | Harvest calendar signals |
| Google Drive | Search files, List recent files | Harvest doc changes |
| BigQuery | Execute query, List tables | Sales intelligence + Mixpanel telemetry |

**Notion and Slack are required.** Everything else is optional — the
orchestrator gracefully skips harvesters whose MCPs aren't available.

## The Second Scheduled Task: DM Monitor

**Name:** Slack DM Monitor
**Repeats:** Every 30 minutes during work hours
**Instructions:**

```
Read and execute {CSOS_PATH}/cadence/slack-dm-monitor.md.
Watch the user's personal Slack DM for account notes.
File to Notion context, confirm in thread.
```

**Required MCP:** Slack (Send message, Read channel), Notion (Update pages)

## How the Orchestrator Works

Every hour:

### 1. Read Configuration
- Read `resources/cadence.md` for schedule rules
- Read `resources/skills/personal/overrides.md` for CSM identity and timezone
- Get current time in CSM's timezone

**Validate overrides has all required fields.** If any are missing, add them
with defaults. This handles the case where the kit was patched after initial
setup and the live overrides file is stale:

| Field | Default if missing |
|-------|-------------------|
| `DISPATCH_TARGET` | `{SLACK_USER_ID}` (from same file) |
| `TELEMETRY_CHANNEL` | `C0B1ZU8QSHE` |
| `DM_MONITOR_ENABLED` | `true` |
| `DM_MONITOR_INTERVAL` | `30m` |

If you add a field, log it: `orchestrator:config:added:{field}={default}`

### 2. Read Today's Changelog
- Read `{LEDGER_PATH}/changelog/{YYYY-MM-DD}.md`
- Extract what has already run: look for `scheduler:completed:{rule}` entries
- This prevents double-runs

### 3. Evaluate Cadence Rules

Check each rule against current time and day:

```
work_hours: 7:00am - 6:00pm
work_days: monday, tuesday, wednesday, thursday, friday

weekdays at 7:30am  → janitor
weekdays at 8:00am  → morning-cycle
weekdays at 8:00am on mondays → expand morning-cycle to weekly
weekdays at 5:00pm  → eod-cycle
fridays at 4:00pm   → eow-cycle
1st monday at 8:30am → monthly-cycle
```

For each rule:
- **Time match?** Is the trigger time within the last hour?
- **Day match?** Weekday, specific day, 1st Monday, etc.
- **Already run?** Check changelog for `scheduler:completed:{name}` today
- If all pass → execute

### 4. Detect Missed Cycles

If the daemon was down yesterday (or longer), detect and backfill:
- Check for missing retro files (EoD retro skill handles backfill logic)
- Check for missing briefing files
- Log what was missed

### 5. Execute Due Cycles

Run the appropriate cycle(s) in order:

---

#### Janitor (10:00am weekdays)

Run `sub/janitor.md`:
- Freshness audit, source validation, inbox check, structural hygiene,
  Notion health, renewal/risk alerts
- Includes `reconcile check` for drift detection
- Posts headline+thread to Slack if warnings found
- Log: `scheduler:completed:janitor`

---

#### Morning Cycle (9:30am weekdays)

**Step 1: Pull from Notion**
- Read all account Context pages from Notion → overwrite local snapshots
- Detect empty/stub pages → set `first-write=true` flag

**Step 2: Harvest**
- Run `sub/harvest-all.md` (calls all available harvesters)
- Record: which harvesters ran, which skipped, total signals found

**Step 3: Compress**
- Run `sub/compress.md` — build patches from signals
- Record: number of patches, accounts touched

**Step 4: Write to Notion**
- Run `sub/notion-sync.md` write mode
- REPLACE mode for first-write, UPDATE mode for existing pages
- Cache locally after Notion write succeeds
- Record: pages written, pages failed, first-writes, duration

**Step 5: Morning Briefing**
- Run `cadence/morning-briefing.md`
- On Mondays: expand to weekly mode (add This Week + Book Health)
- Save to Ledger, push to Notion Workspace Ledger
- Record: critical items, action items, accounts mentioned

**Step 6: Post to Slack**
- Post full briefing as top-level message (the only skill that posts full)
- Log: `scheduler:completed:morning-cycle`

**Step 7: Fire Telemetry**
- Read `sub/telemetry.md` for the event format
- Read TELEMETRY_CHANNEL from overrides. Post THREE events to TELEMETRY_CHANNEL via `slack_send_message`, each as a ```json code block:
  1. `csa_os:cycle_completed` with: cycle="morning", accounts, signals, patches, notion_writes, notion_errors, harvesters_run, harvesters_skipped, duration_seconds
  2. `csa_os:briefing_posted` with: type="morning", dispatch, critical_items, action_items, accounts_mentioned
  3. `csa_os:notion_sync` with: mode="write", accounts_synced, accounts_failed, first_writes, conflicts, duration_seconds
- If any post fails, log the failure and continue. Never block on telemetry.

---

#### EoD Cycle (4:00pm weekdays)

**Step 1: Pull from Notion**
- Read all account Context pages → overwrite local snapshots

**Step 2: EoD Retrospective**
- Run `cadence/eod-retrospective.md`
- Includes backfill for missed days
- Write V/R/G Log entries to Notion
- Write context patches to Notion
- Record: accounts touched, patches, backfilled days, notion writes, duration

**Step 3: Post to Slack**
- Headline: `🌙 EoD — {N} touched, {N} carrying, tomorrow: {priority}`
- Detail in thread
- Log: `scheduler:completed:eod-cycle`

**Step 4: Fire Telemetry**
- Read `sub/telemetry.md` for the event format
- Read TELEMETRY_CHANNEL from overrides. Post TWO events to TELEMETRY_CHANNEL:
  1. `csa_os:cycle_completed` with: cycle="eod", accounts, signals=0, patches, notion_writes, backfilled_days, duration_seconds
  2. `csa_os:briefing_posted` with: type="eod", dispatch, accounts_mentioned

---

#### EoW Cycle (Thursday 3:00pm)

**Step 1: Pull from Notion**
**Step 2: EoW Retrospective**
- Run `cadence/eow-retrospective.md`
- 5-day changelog read, cross-account patterns
- Write V/R/G Log entries to Notion

**Step 3: Post to Slack**
- Headline+thread
- Log: `scheduler:completed:eow-cycle`

**Step 4: Fire Telemetry**
- Read `sub/telemetry.md` for the event format
- Read TELEMETRY_CHANNEL from overrides. Post TWO events to TELEMETRY_CHANNEL:
  1. `csa_os:cycle_completed` with: cycle="eow"
  2. `csa_os:briefing_posted` with: type="eow"

---

#### Monthly Cycle (1st Sunday 9:30am, after morning cycle)

**Step 1: Monthly Consolidation**
- Run `cadence/monthly-consolidation.md`
- 30-day review, book health, V/R/G sourcing

**Step 2: Post to Slack**
- Headline+thread
- Log: `scheduler:completed:monthly-cycle`
- Archive old changelogs

**Step 3: Fire Telemetry**
- Read `sub/telemetry.md` for the event format
- Read TELEMETRY_CHANNEL from overrides. Post TWO events to TELEMETRY_CHANNEL:
  1. `csa_os:cycle_completed` with: cycle="monthly"
  2. `csa_os:briefing_posted` with: type="monthly"

---

### 6. Handle Slack Commands

Between scheduled cycles, check for commands in the dispatch DM.
Use `roles/slack-dispatch.md` to parse:

| Command | Action |
|---------|--------|
| `briefing` | Run morning briefing |
| `retro` | Run EoD retro |
| `weekly` | Run EoW retro |
| `refresh {account}` | Pull + harvest + compress + write for one account |
| `refresh all` | Full morning cycle |
| `sync` | Reconcile check + fix |
| `status` | Post system status (headline+thread) |
| `help` | Post command list |

After processing any command, fire telemetry:
- Read `sub/telemetry.md` for the event format
- Read TELEMETRY_CHANNEL from overrides. Post ONE event to TELEMETRY_CHANNEL:
  `csa_os:slack_interaction` with: interaction_type="command", command="{name}"

### 7. Log Tick

```
## {HH:MM} — orchestrator:tick

- **Type:** orchestrator:tick
- **Detail:** Evaluated {N} rules. Executed: {list}. Skipped: {list} (already run | not due).
```

### 8. Error Telemetry

If ANY step in ANY cycle fails, fire telemetry immediately (don't wait for step 7):
- Read `sub/telemetry.md` for the event format
- Read TELEMETRY_CHANNEL from overrides. Post ONE event to TELEMETRY_CHANNEL:
  `csa_os:error` with: skill="{name}", error_type="{type}", error_message="{first 200 chars}", cycle="{current}"
- Then continue with the next step if possible, or abort the cycle if the error is fatal.

## Status Command

When someone sends `status`:

Top-level:
```
📊 Status — {N} accounts synced, next: {cadence} at {time}
```

Thread:
```
Dispatch: Personal DM
DM Monitor: Active (30m)
Last cycle: {time} — {what ran}
Accounts: {N} total, {N} stale (>7d)
Today: {N} signals, {N} patches
Notion: {N} in sync, {N} drifted

Scheduled:
  ✅ janitor — {time}
  ✅ morning-cycle — {time}
  ⏳ eod-cycle — due at 5:00pm
  ○ eow-cycle — friday only
  ○ monthly — 1st monday only
```

## Migration: Deleting Old Scheduled Tasks

If you have individual scheduled tasks from v3.6 or earlier, delete them:
- ❌ Janitor (delete)
- ❌ Morning cycle (delete)
- ❌ Eod cycle (delete)
- ❌ Monthly cycle (delete)
- ❌ Csa rubric weekly (delete if subsumed)
- ✅ Os orchestrator (KEEP — this is the one)
- ✅ Slack dm monitor (KEEP — separate 30m task)
