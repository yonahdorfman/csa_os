---
name: Harvest All
version: 1.1
last_updated: 2026-07-16
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

## Source Priority: BQ First

For any harvester that has both a BigQuery path and an MCP path, **BigQuery is
always the default read** — it's cheaper, faster, has no rate limits, and doesn't
depend on a live MCP connection. Only fall back to the MCP for the narrow
real-time gap that harvester's own skill file documents (e.g. today's calls
that haven't landed in BQ yet). Never reach for the MCP first "just because
it's connected." This is already how `harvest-gong.md` is written; treat it as
the house rule for every harvester, present and future — including Pylon,
whenever a BQ table for it exists (there isn't one yet as of 2026-07-16).

## Execution

Run these harvesters in sequence. Each checks its own MCP/BQ availability
and skips gracefully if unavailable:

1. `sub/harvest-gmail.md` (requires: Gmail MCP)
2. `sub/harvest-slack.md` (requires: Slack MCP) — **has its own batching/rotation
   design (Coverage Rotation + priority set); always run its batch, never skip for
   scope/cost. Only a missing Slack MCP is a valid reason to skip this line.**
3. `sub/harvest-calendar.md` (requires: Google Calendar MCP)
4. `sub/harvest-notion.md` (requires: Notion MCP)
5. `sub/harvest-drive.md` (requires: Google Drive MCP)
6. `sub/harvest-bq.md` (requires: BigQuery MCP)
7. `sub/harvest-mixpanel.md` (requires: BigQuery or Mixpanel MCP)
8. `sub/harvest-gong.md` (requires: BigQuery; optional: Gong MCP) ← **the only call-intelligence source in the harvest — BQ first, MCP only for today's not-yet-ingested calls**
9. `sub/harvest-pylon.md` (requires: Pylon MCP) ← **the only support-ticket source in the harvest**
10. `sub/harvest-asana.md` (requires: Asana MCP)

**Not called by default (standby):**

- `sub/harvest-clari.md` — superseded by Gong for call intelligence. Kept on
  disk, not deleted, in case Gong data ever needs cross-checking against the
  old Clari dataset. Do not add it back to the sequence above without asking
  first; if reactivated, treat it as BQ-first same as Gong.
- `sub/harvest-zendesk.md` — superseded by Pylon for support tickets. Kept on
  disk for the same reason. Note: the query in that file targets
  `sales_intelligence.zendesk_tickets_enriched`, which no longer exists
  (dropped post-migration) — `zendesk_tickets_raw` and
  `zendesk_tickets_redacted` are the current tables, so the query needs
  updating before this harvester could be reactivated.

Since these two are never invoked, they should not appear in `HARVESTERS_DONE`,
`HARVESTERS_MISSING`, or any per-cycle skip log — there's nothing to skip if
it's never called.

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
  Gmail: {N} | Slack: {N} | Calendar: {N} | Notion: {N} | Drive: {N} | BQ: {N} | Gong: {N} | Pylon: {N} | Asana: {N}
```

## Notes

- If ALL harvesters skip (no MCPs connected), return an empty signal list.
  The compress step handles empty input gracefully.
- The assistant.md morning cycle references running harvesters "in parallel
  where possible." In Cowork, this means running them sequentially but as
  fast as possible. True parallelism depends on Cowork's execution model.
- Clari and Zendesk are standby harvesters as of 2026-07-16 (see "Not called
  by default" above) — they're intentionally absent from the execution list,
  the log line, and `HARVEST_REQUIRED` in orchestrator.md. This isn't a skip
  condition to monitor; it's the current design.
