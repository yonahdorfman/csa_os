# CSA OS — Scheduled Task Definitions

These are the EXACT instructions to paste into each Cowork scheduled task.
They are thin pointers that invoke skill files. All logic lives in the skill
files, which can be patched without touching the scheduled tasks.

**Rule:** Scheduled tasks contain ONLY routing logic. They tell Claude WHICH
file to read and execute. They never contain the execution logic itself.
If you need to change behavior, patch the skill file — not the task.

---

## Task 1: CSA OS Orchestrator

**Name:** CSA OS Orchestrator
**Repeats:** Hourly during work hours (Sun–Thu, 9am–6pm GMT+3 — see `resources/cadence.md`)
**Folder:** {your CSOS folder path}

### Instructions (paste this):

```
You are the CSA OS orchestrator. Your job is to read skill files and execute them.

1. Read `resources/skills/personal/overrides.md` for identity and configuration.
2. Read `resources/cadence.md` for the schedule.
3. Read `roles/orchestrator.md` and execute it as written.

The orchestrator skill file contains all logic: which cycles to run, when,
how to detect missed cycles, how to chain sub-skills, and how to fire telemetry.
Follow it step by step.

If orchestrator.md references another skill file (e.g., "Run sub/harvest-all.md"
or "Run cadence/morning-briefing.md"), read that file and execute it too.

Do not improvise or skip steps. If a step says "Post to C0B1ZU8QSHE using
slack_send_message", that is a real action — do it.

All context writes go to Notion first, then cache locally. Notion is the
source of truth. If a Notion write fails, do not write locally.
```

### MCP Permissions (Always Allowed):

- Notion: Read pages, Update pages, Create pages, Search
- Slack: Send message, Read channel, Read user profile, Search
- Gmail: Search threads, Get thread
- Google Calendar: List events, Get event
- Google Drive: Search files, List recent files, Read file content
- BigQuery: Execute query, List tables, Describe table

---

## Task 2: Slack DM Monitor

**Name:** Slack DM Monitor
**Repeats:** Every hour during work hours (Sun–Thu, 9am–6pm GMT+3 — see `resources/cadence.md`)
**Folder:** {your CSOS folder path}

### Instructions (paste this):

```
You are the CSA OS DM monitor. Your job is to read a skill file and execute it.

1. Read `resources/skills/personal/overrides.md` for identity and configuration.
2. Read `cadence/slack-dm-monitor.md` and execute it as written.

The skill file contains all logic: how to read DMs, classify messages, file
to Notion context, confirm in thread, log to changelog, and fire telemetry.
Follow it step by step.

If the skill references another file (e.g., "Read sub/telemetry.md"), read
that file and follow its instructions too.

Do not improvise or skip steps. If a step says "Post to C0B1ZU8QSHE using
slack_send_message", that is a real action — do it.

All context writes go to Notion first. Notion is the source of truth.
```

### MCP Permissions (Always Allowed):

- Notion: Read pages, Update pages, Search
- Slack: Send message, Read channel, Read user profile

---

## Why This Pattern Works

1. **Patchable.** Change the skill file → next run picks it up. No task editing.
2. **Auditable.** The skill file is versioned. You can diff what changed.
3. **Consistent.** Every CSA runs the same skill files. No per-person drift.
4. **Debuggable.** If something breaks, check the skill file — not the task description.

## What NOT to Put in Task Instructions

- ❌ Execution logic ("For each account, harvest signals from Slack...")
- ❌ Output formats ("Post briefing as: ☀️ Morning Briefing — ...")
- ❌ Cycle definitions ("At 8am run morning cycle, at 5pm run eod...")
- ❌ Telemetry calls ("Post JSON to C0B1ZU8QSHE...")
- ❌ Account names or hardcoded data
- ❌ Version-specific behavior

All of that lives in the skill files. The task just says "read the file, do what it says."

## Migration from v3.6 Tasks

Delete all existing scheduled tasks except the orchestrator and DM monitor.
Then update those two with the instructions above.

Tasks to delete:
- Janitor
- Morning cycle
- Eod cycle
- Monthly cycle
- Csa rubric weekly
- Any other per-cadence tasks

The orchestrator subsumes all of them. It reads cadence.md to know what's due.
