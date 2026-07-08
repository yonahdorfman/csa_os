# Hygiene Report — 2026-07-01

## Job 1: Context Freshness

✅ Fresh (≤7d): 2 accounts (Elementor, HoneyBook)
⚠️ Stale (7–14d): 20 accounts — AT&T Israel, Altshuler Shaham, AutoDS IL, DuxGroup, Fibi - Israeli International Bank, Fireblocks, Gamdom, Guard.io, HAAT Delivery, Kape Technologies PLC (Israel), Nanit, PayBox - Discount Bank, PeerPlay, Riverside.fm, SimilarWeb, Solaredge, StakeMate, Whalo, runwayer.com, shortical.com
🔴 Critical (>14d): 25 accounts — Appcharge LTD, Base44, BlueThrone, Bookaway, ClickSoftware, Duda, Ferryhopper, Forex Club International LLC, GETT, GameStory Israel, Lemonade, Loora.AI, Movement-Group, MyHeritage Ltd. main account, Nexus Mods, Pango Pay & Go ltd, Papaya Global {Formerly Azimo}, RubyLabs, Simply, Stillfront Group AB, Tabnine (Previously Codota), Taboola, Talon Cyber Security, boinkers.io BY Acid Labs, mbg-digital.com

Total: 47 accounts.

## Job 2: Source Validation

✅ Notion Accounts DB accessible, spot-check OK (AT&T Israel Context page — full body renders, matches local cache).
✅ Slack DM (dispatch channel) accessible.
✅ Calendar accessible (today's events pulled successfully).
✅ Gmail accessible (recent threads pulled successfully).
✅ BigQuery accessible (gong_calls, account_plans_current queried successfully).
✅ Gong MCP accessible (recent calls pulled).
✅ Google Drive accessible (recent files pulled).
⚠️ Mixpanel MCP: `Get-Business-Context` failed — missing `business_context` scope. MCP needs reconnect with proper scopes. BQ remains the working fallback for Mixpanel telemetry.

## Job 3: Inbox

Pending: 2 items (neither moved to `.processed/`)
- `2026-06-30-dm-notes.md` — infra note (Gong setup confirmed complete), already classified/actioned, not filed to .processed
- `2026-05-27-dm-notes.md` — unmatched note ("walo" — likely truncated reference to Whalo), still needs manual triage/filing

## Job 4: Structure + Sync

✅ 47/47 accounts have complete file sets (_context.md + _MANIFEST.md present for all)
✅ No orphaned folders detected
✅ Registry covers all 47 accounts (spot-checked AT&T Israel, Accounts DB — both resolve)

Drift check:
✅ AT&T Israel spot-checked: local and Notion content match (both carry the 2026-06-30 EoD "renewal date passed" entry as the latest entry). No drift detected in spot-check.
⚠️ Full 47-account reconcile not run this tick (time-boxed); no drift signals surfaced by harvest.

## Job 5: Notion Health

✅ Accounts DB accessible (schema: Name, AE, ARR, Health, Last Updated, Region, Renewal Date, Slack Channel)
✅ 1/1 spot-checked Context page OK (AT&T Israel — full body renders correctly)
⚠️ No full spot-check of all 47 registry entries this tick — recommend `/reconcile check` run in a dedicated cycle

## Job 6: Upcoming Renewals + Risk Flags

🔴 **AT&T Israel — renewal PASSED (2026-06-30), $63,205 ARR, no outcome confirmed.** Legal hold was last confirmed active Jun 22. No close, no extension, no update captured since. This is a carry-over critical item from yesterday — requires immediate escalation to Tal Huberman / Ofer Polivoda today.
🔴 Lemonade — renewal date passed (2026-05-31) but account context confirms recently closed/renewed (no action needed — stale renewal field, not a real risk).
⚠️ Renewal <60d: Elementor ($107K, 2026-07-31, Stage 5 Signing), MyHeritage ($110K, 2026-07-31), GameStory Israel (2026-08-01)
⚠️ Overdue items: 35 total across the book, incl. AT&T Israel (2 overdue), HoneyBook (6 overdue), Ferryhopper (3), PeerPlay (3), Nexus Mods (3), Talon Cyber Security (3), runwayer.com (3)

## Summary

- Warnings: 13 (25 critical-stale accounts flagged as one item, 20 stale as one item, AT&T renewal-passed, Mixpanel MCP scope issue, 2 inbox items pending, 35 overdue items, 3 renewals <60d, incomplete full reconcile)
- Stale: 20 | Critical: 25 | Broken sources: 1 (Mixpanel MCP scope)
- Renewals <60d: 3 (+1 passed unresolved: AT&T)
- Overdue items: 35
