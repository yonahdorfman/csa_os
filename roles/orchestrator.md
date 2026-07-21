---
name: Orchestrator
version: 1.4
last_updated: 2026-07-21
category: role
description: >
  THE single scheduled Cowork task (besides DM monitor). Runs hourly.
  Evaluates cadence rules, detects missed cycles, executes what's due.
  There should be NO other scheduled tasks for individual skills.
requires_mcp: [Notion, Slack]
optional_mcp: [Gmail, Google Calendar, Google Drive, BigQuery, Mixpanel MCP, Statisfy]
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
**Repeats:** Hourly during work hours (see `resources/cadence.md` — currently Sun–Thu, 9am–6pm GMT+3)
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
| Statisfy | Read objects, Read relationships | Live CRM enrichment (health scorecard, pipeline, contacts) — optional |

**Notion and Slack are required.** Everything else is optional — the
orchestrator gracefully skips harvesters whose MCPs aren't available.

## The Second Scheduled Task: DM Monitor

**Name:** Slack DM Monitor
**Repeats:** Every hour during work hours
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
| `DM_MONITOR_INTERVAL` | `60m` |

If you add a field, log it: `orchestrator:config:added:{field}={default}`

### 2. Read Today's Changelog
- Read `{LEDGER_PATH}/changelog/{YYYY-MM-DD}.md`
- Extract what has already run: look for `scheduler:completed:{rule}` entries
- This prevents double-runs

**Also extract per-harvester completion markers** (see Harvest section):
- Look for `harvest:{source}:completed` entries (e.g. `harvest:bq:completed`)
- Build a list: `HARVESTERS_DONE` = all sources with a completion marker today
- This is used to detect partial harvests in Step 3

### 3. Evaluate Cadence Rules

Check each rule against current time and day:

This is a format example only — always read the live values from `resources/cadence.md`, currently:

```
work_hours: 9:00am - 6:00pm
work_days: sunday, monday, tuesday, wednesday, thursday

weekdays at 9:30am → janitor
weekdays at 9:00am → morning-cycle
weekdays at 9:30am on sundays → expand morning-cycle to weekly
weekdays at 4:00pm → eod-cycle
thursdays at 3:00pm → eow-cycle
1st sunday at 9:30am → monthly-cycle
```

For each rule:
- **Time match?** Is the trigger time within the last hour?
- **Day match?** Weekday, specific day, 1st Monday, etc.
- **Already run?** Check changelog for `scheduler:completed:{name}` today
- If all pass → execute

**Partial harvest check (runs every tick, independent of cycle schedule):**

Define `HARVEST_REQUIRED` = [gmail, slack, calendar, bq, gong, mixpanel]
These are the sources that must complete for a harvest to be considered full.
Notion, Drive, Asana, Pylon, and Statisfy are optional — skipping them is not an error.
Clari and Zendesk are standby sources (see harvest-all.md's "Not called by
default" section) — they are never invoked, so they should never appear in
`HARVESTERS_DONE`, `HARVESTERS_MISSING`, or a skip log entry at all.

**Valid skip reasons for a `HARVEST_REQUIRED` source are `skipped:mcp_unavailable`
only.** There is no `skipped:scope` reason, and one must never be logged again — "the
book is too big to scan this tick" is not a valid justification for a source that
defines its own batching/rotation mechanism (currently: Slack, see harvest-slack.md's
Coverage Rotation). That mechanism exists precisely so the harvester never needs to be
skipped wholesale for cost reasons — run its batch every cycle without exception, even
if recent cycles skipped it. Do not treat a prior cycle's skip as precedent; re-evaluate
each harvester's current skill file every tick instead of carrying forward a past
judgment call.

If `scheduler:completed:morning-cycle` is present today but `HARVESTERS_DONE`
does not contain all of `HARVEST_REQUIRED`:
- Identify `HARVESTERS_MISSING` = HARVEST_REQUIRED minus HARVESTERS_DONE
- If all missing harvesters' MCPs are now available → run **harvest-catchup**:
  1. Run only the missing harvesters from `sub/harvest-all.md`
  2. Run `sub/compress.md` on new signals only
  3. Run `sub/notion-sync.md` write mode for affected accounts
  4. Log each recovered harvester: `harvest:{source}:completed`
  5. Post to Slack DM: `⚡ Harvest catch-up: recovered {sources} ({N} new signals)`
  6. Log: `scheduler:completed:harvest-catchup`
- If MCPs are still unavailable → skip silently, log `harvest:{source}:skipped:mcp_unavailable`

This means a session interruption mid-harvest is automatically recovered on the
next tick, without re-running the full morning cycle.

**Pending Notion writes check (runs every tick, independent of cycle schedule):**

Compare today's changelog:
- Accounts with a `compress:wrote:{account}` / `compress:context-update` entry, or any
  entry saying a write was "deferred", "batched", or "scheduled for a later cycle"
- vs accounts with a matching `notion-sync:write` success entry today

If any account has patches (or a deferral note) without a completed Notion write →
run `sub/notion-sync.md` write mode NOW for exactly those accounts, and log
`notion-sync:write` per account. **There is no "later batch write" — a deferral
note is a bug, not a plan.** (2026-07-21: a "batched write scheduled for later
cycle (45 accounts pending)" note was logged in the morning and the write never
ran; this check exists so that can't recur.)

### 4. Detect Missed Cycles

If the daemon was down yesterday (or longer), detect and backfill:
- Check for missing retro files (EoD retro skill handles backfill logic)
- Check for missing briefing files
- Log what was missed

### 5. Execute Due Cycles

Run the appropriate cycle(s) in order:

---

#### Janitor (9:30am weekdays)

Run `sub/janitor.md`:
- Freshness audit, source validation, inbox check, structural hygiene,
  Notion health, renewal/risk alerts
- Includes `reconcile check` for drift detection
- Posts headline+thread to Slack if warnings found
- Log: `scheduler:completed:janitor`

---

#### Morning Cycle (9:00am weekdays)

**Step 1: Pull from Notion (DELTA pull — not all 47 pages)**
- Query the Accounts DB for pages whose `Last edited time` is newer than the last
  successful pull (yesterday's cycle; fall back to 24h if unknown) — pull ONLY those
  page bodies and overwrite their local snapshots. All other accounts keep their
  local cache. Most days this is 0–5 pages, not 47; a full-book pull is only for
  first run or when `reconcile check` flags drift. This is the single biggest
  token saver in the system.
- Detect empty/stub pages → set `first-write=true` flag

**Step 2: Harvest**
- Run `sub/harvest-all.md` (calls all available harvesters)
- For each harvester that completes, immediately log: `harvest:{source}:completed`
  (e.g. `harvest:gmail:completed`, `harvest:bq:completed`)
- For each harvester that is skipped (MCP unavailable), log: `harvest:{source}:skipped:mcp_unavailable`
- Do NOT log `harvest-all:completed` — the per-source markers replace it
- Record: which harvesters ran, which skipped, total signals found

**Step 3: Compress**
- Run `sub/compress.md` — build patches from signals
- Record: number of patches, accounts touched

**Step 4: Write to Notion**
- Run `sub/notion-sync.md` write mode for the accounts compress patched THIS cycle
  (typically 1–10 accounts) — NOT the whole book. Unpatched accounts have nothing
  to write.
- REPLACE mode for first-write, UPDATE mode for existing pages
- Cache locally after Notion write succeeds
- Record: pages written, pages failed, first-writes, duration
- **This step is not deferrable.** Do not log "batched for a later cycle" — run it
  now, in the same cycle that produced the patches.

**Step 5: Morning Briefing**
- Run `cadence/morning-briefing.md`
- On Mondays: expand to weekly mode (add This Week + Book Health)
- Save to Ledger, push to Notion Workspace Ledger
- Record: critical items, action items, accounts mentioned

**Step 6: Post to Slack**
- Post full briefing as top-level message (the only skill that posts full)
- Log `scheduler:completed:morning-cycle` — **ONLY if Steps 2–4 actually executed**
  (harvest batch run per each harvester's own spec, compress run, notion-sync write
  done for every patched account). Posting the briefing does NOT complete the cycle.
  If any of Steps 2–4 were skipped or partial, do NOT log the completion marker —
  the per-tick partial-harvest and pending-writes checks will finish the job on the
  next tick, and they key off the marker's absence.

**Step 7: Fire Telemetry**
- Read `sub/telemetry.md` for the event format
- Read TELEMETRY_CHANNEL from overrides. Post THREE events to TELEMETRY_CHANNEL via `slack_send_message`, each as a ```json code block:
  1. `csa_os:cycle_completed` with: cycle="morning", accounts, signals, patches, notion_writes, notion_errors, harvesters_run, harvesters_skipped, duration_seconds
  2. `csa_os:briefing_posted` with: type="morning", dispatch, critical_items, action_items, accounts_mentioned
  3. `csa_os:notion_sync` with: mode="write", accounts_synced, accounts_failed, first_writes, conflicts, duration_seconds
- If any post fails, log the failure and continue. Never block on telemetry.

---

#### EoD Cycle (4:00pm weekdays)

**Step 1: Pull from Notion (DELTA pull)**
- Same as the morning cycle: pull only pages edited in Notion since the last
  successful pull. Never re-pull the full book on a routine cycle.

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
- **Harvest:** {HARVESTERS_DONE} complete, {HARVESTERS_MISSING} missing.
```

Include the harvest line on every tick so any partial harvest is visible at a glance.

**Changelog brevity (token control):** one tick = one entry, ≤6 lines. Do not write
additional `orchestrator:status` / `status-final` entries restating the same cadence
evaluation (2026-07-21 had four near-identical evaluations logged in 20 minutes).
Detailed narration belongs in the skill-specific entries (harvest, compress, sync),
and those should be summaries, not transcripts. Every line written here is re-read
by every subsequent tick that day — verbosity compounds.

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
DM Monitor: Active (60m)
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
- ✅ Slack dm monitor (KEEP — separate 60m task)
