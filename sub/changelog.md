---
name: Changelog
version: 1.0
last_updated: 2026-04-21
category: sub-skill
inputs:
  - LEDGER_PATH: "Base path to Ledger"
  - CSM_TIMEZONE: "IANA timezone"
invoke:
  - Called as side effect by every other skill
---

# Changelog

Append-only daily activity log at `{LEDGER_PATH}/changelog/{YYYY-MM-DD}.md`.

Every skill appends here after execution. The EoD retro and scheduler read from here
instead of re-scanning sources.

## Entry Format

```markdown
## {HH:MM} — {source}:{action}

- **Account:** {account_name or "book-level"}
- **Type:** {entry_type}
- **Detail:** {one-line summary}
- **Refs:** {optional: page IDs, thread links, file paths}
```

## Entry Types

| Type | When Used |
|------|-----------|
| `harvest:gmail` | Gmail harvester completed |
| `harvest:slack` | Slack harvester completed |
| `harvest:calendar` | Calendar harvester completed |
| `harvest:notion` | Notion harvester completed |
| `harvest:drive` | Drive harvester completed |
| `harvest:bq` | BQ harvester completed |
| `compress:context-update` | Compress wrote patches to _context.md |
| `notion-sync:pulled` | Notion → local sync |
| `notion-sync:pushed` | Local → Notion sync |
| `notion-sync:conflict` | Sync conflict detected |
| `scheduler:completed` | Scheduled cadence completed |
| `briefing:generated` | Morning briefing produced |
| `retro:generated` | Retrospective produced |
| `command:received` | Slack command processed |
| `error` | Something failed |

## File Management

- One file per day: `{YYYY-MM-DD}.md`
- Created on first append of the day
- Files older than 90 days can be archived (monthly consolidation does this)

## Reading the Changelog

Skills that read the changelog (retros, briefings, scheduler):
- Read today's file for "what's run today"
- Read last N days for retro summaries
- Check for `scheduler:completed:{rule_name}` to avoid double-runs
