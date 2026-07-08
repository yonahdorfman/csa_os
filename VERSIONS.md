# CSA OS — Version History

## v4.1.6 — Fix Command Routing (May 1, 2026)

- Added `janitor` and `hygiene` to DM monitor command table (was missing)
- Changed command execution language from "ROUTE to orchestrator" (ambiguous —
  Claude interpreted as "tell someone else") to "Read and execute this skill
  file immediately. Do not skip." (unambiguous directive)
- Removed `morning` as a standalone keyword (too broad — matches "good morning")
- Added `morning briefing` and `run briefing` as compound matches

## v4.1.5 — DM Monitor Routes Commands + Accepts Threads (May 1)
## v4.1.4 — Bugfixes (May 1)
## v4.1.3 — Overrides Self-Heal + Channel Consolidation (May 1)
## v4.1.2 — Structured Telemetry Channel (May 1)
## v4.1.1 — Dual Dispatch (April 30)
## v4.1.0 — Slack Dispatch Fix (April 30)
## v4.0.0 — Production Release (April 30)
