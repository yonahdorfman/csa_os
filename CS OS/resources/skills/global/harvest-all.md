---
name: Harvest All
version: 1.0
last_updated: 2026-04-22
category: sub-skill
description: >
  Wrapper skill that runs all available harvesters and combines their
  output into a single signal list for the compress step. Referenced
  by cadence.md as "harvest-all".
inputs:
  - ACCOUNTS_PATH: "Base path to accounts"
  - CSM_NAME: "Your name"
  - SLACK_USER_ID: "Your Slack user ID"
  - CSM_TIMEZONE: "IANA timezone"
  - LOOKBACK_HOURS: "default: 24"
output: Combined signal list from all sources
invoke:
  - Called by assistant during morning/evening cycles
  - Referenced in cadence.md as "harvest-all"
---

# Harvest All

Runs every available harvester and combines their output. This is what
cadence.md means when it says `→ harvest-all →`.

## Execution

Run these harvesters in sequence. Each checks its own MCP availability
and skips gracefully if unavailable:

1. `sub/harvest-gmail.md` (requires: Gmail MCP)
2. `sub/harvest-slack.md` (requires: Slack MCP)
3. `sub/harvest-calendar.md` (requires: Google Calendar MCP)
4. `sub/harvest-notion.md` (requires: Notion MCP)
5. `sub/harvest-drive.md` (requires: Google Drive MCP)
6. `sub/harvest-bq.md` (requires: BigQuery MCP)
7. `sub/harvest-mixpanel.md` (requires: BigQuery or Mixpanel MCP)
8. `sub/harvest-gong.md` (requires: BigQuery; optional: Gong MCP) ← **primary call source**
9. `sub/harvest-clari.md` (requires: BigQuery; optional: Clari Copilot MCP) ← fallback for pre-migration calls
10. `sub/harvest-pylon.md` (requires: Pylon MCP) ← **primary support ticket source**
11. `sub/harvest-zendesk.md` (requires: BigQuery) ← fallback for pre-migration (historical) tickets only
12. `sub/harvest-asana.md` (requires: Asana MCP)

## Output

Combine all harvester outputs under a single header:

```markdown
## Harvest Complete — {date}

**Sources scanned:** {list of harvesters that ran}
**Sources skipped:** {list of harvesters that skipped + reason}
**Total signals:** {N}

---

{gmail harvest output}

---

{slack harvest output}

---

{calendar harvest output}

---

{notion harvest output}

---

{drive harvest output}

---

{bq harvest output}

---

{asana harvest output}
```

This combined output is passed to `sub/compress.md` as `HARVEST_OUTPUT`.

## Logging

One changelog entry for the batch:
```
## {HH:MM} — harvest-all:completed

- **Type:** harvest:completed
- **Detail:** {N} sources scanned, {N} skipped. {N} total signals.
  Gmail: {N} | Slack: {N} | Calendar: {N} | Notion: {N} | Drive: {N} | BQ: {N} | Gong: {N} | Clari: {N} | Pylon: {N} | Zendesk: {N} | Asana: {N}
```

## Notes

- If ALL harvesters skip (no MCPs connected), return an empty signal list.
  The compress step handles empty input gracefully.
- The assistant.md morning cycle references running harvesters "in parallel
  where possible." In Cowork, this means running them sequentially but as
  fast as possible. True parallelism depends on Cowork's execution model.
