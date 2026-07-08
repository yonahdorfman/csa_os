# Hygiene Report — 2026-07-06 (Monday)

## Job 1: Context Freshness

✅ Fresh (≤7d): 10 accounts — AT&T Israel, Elementor, GameStory Israel, HoneyBook, Lemonade, MyHeritage Ltd. main account, PayBox - Discount Bank, PeerPlay, Tabnine (Previously Codota), shortical.com
⚠️ Stale (7-14d): 4 accounts — DuxGroup (14d), Gamdom (14d), Guard.io (14d), Nanit (14d)
🔴 Critical (>14d): 31 accounts — Altshuler Shaham, Appcharge LTD, AutoDS IL, Base44, BlueThrone, Bookaway, ClickSoftware, Duda, Ferryhopper, Fibi - Israeli International Bank, Fireblocks (stub, never harvested), Forex Club International LLC, GETT, HAAT Delivery, Kape Technologies PLC (Israel), Loora.AI, Movement-Group, Nexus Mods, Pango Pay & Go ltd, Papaya Global {Formerly Azimo}, Riverside.fm, RubyLabs, SimilarWeb, Simply, Solaredge, StakeMate, Stillfront Group AB, Taboola, Talon Cyber Security, Whalo, boinkers.io BY Acid Labs, mbg-digital.com, runwayer.com

**Systemic pattern (repeats from prior runs):** morning-cycle harvest/compress is consistently scoped to only the "top 10 by urgency" accounts for tool-budget reasons. This means ~31 of 47 accounts (66%) haven't had a genuine context refresh in 2+ weeks — freshness is structurally capped, not a one-off gap. Recommend a dedicated full-book harvest-catchup session (flagged in this same form on 2026-07-05 and 2026-07-06 morning cycle) rather than continuing to accept this as steady-state.

## Job 2: Source Validation

✅ Notion spot-check: 2/2 pages resolved correctly (DuxGroup, PayBox - Discount Bank) — both have Context/Manifest/Contacts children intact.
⚠️ Slack, Drive, BQ live source pings not exercised this tick (folded into the "skipped:budget" harvesters already logged at 09:21 — not re-tested independently by janitor to avoid duplicate calls).

## Job 3: Inbox

Pending: 2 items (both older, not yet in `.processed/`)
• `2026-05-27-dm-notes.md` — unmatched by filename, needs manual triage (40 days old)
• `2026-06-30-dm-notes.md` — unmatched by filename, needs manual triage (6 days old)

## Job 4: Structure + Sync

✅ 45/45 local account folders have both `_context.md` and `_MANIFEST.md`
⚠️ Missing registry entry: **Fireblocks** — no `ACCOUNT:Fireblocks=...` line in notion-page-registry.md. Consistent with its stub-only context ("No Slack channels configured, no BQ pull yet") — this account was likely added to the local folder tree but never run through initial setup/add-account.
⚠️ Naming mismatch: local folder **"Papaya Global {Formerly Azimo}"** (curly braces) vs. registry entry **"Papaya Global (Formerly Azimo)"** (parentheses) — any lookup keyed on exact folder name will fail to resolve against the registry. This is very likely why Papaya Global hasn't been touched since 2026-06-03 (33 days, in the critical bucket above) — silent resolution failures rather than genuine low priority.
✅ cadence.md present and parseable
✅ overrides.md has no unfilled `{PLACEHOLDER}` values

Drift check (light):
✅ 2/2 spot-checked accounts (DuxGroup, PayBox) in sync between local cache and Notion child pages
⚠️ Notion **database property** "Last Updated" on the PayBox parent page still reads 2026-06-21, even though the Context child page and local `_context.md` were both updated today (2026-07-06) per this morning's notion-sync write. The child-page write appears to not be propagating to the parent DB rollup/property — worth a `/reconcile fix` pass to confirm this isn't happening across all 7 accounts touched this morning.

## Job 5: Notion

✅ Accounts DB accessible
✅ 2/2 spot-checked pages OK (DuxGroup, PayBox - Discount Bank)
⚠️ Registry gap noted above (Fireblocks) counts as a stale/broken registry entry from the opposite direction (local exists, registry doesn't)

## Job 6: Alerts

🔴 Renewal lapsed/passed: **AT&T Israel** (expired Jun 30, now 6 days lapsed — already the #1 item in this morning's briefing)
🔴 Renewal lapsed/passed: **DuxGroup** (Notion property shows Renewal Date 2026-06-30 — also 6 days lapsed, and NOT currently surfaced in the morning briefing or First Things First. DuxGroup is also in the Stale bucket above, 14 days since last context touch — this looks like a live gap: a lapsed renewal sitting in a stale account that hasn't been reviewed in 2 weeks.)
⚠️ Critical health / high risk (from context + Notion, top-10-reviewed accounts): AT&T Israel, Lemonade, MyHeritage, PayBox - Discount Bank, PeerPlay, Tabnine, shortical.com — all Health: Low/Medium + Risk: High, all cross-referenced in this morning's briefing.
⚠️ Overdue items >7 days: MyHeritage funnel/cohort discrepancy (55+ days, per this morning's briefing) — no other overdue items surfaced from the local context files this pass (most critical-bucket accounts don't carry structured due-dates locally, another symptom of the stale-context issue above).
⚠️ Renewals <60d not otherwise assessed this pass: full-book renewal-date sweep would require a live BQ/Notion query across all 47 accounts (only 10 were checked); recommend folding this into the recommended harvest-catchup session.

## Summary

**3 warnings worth acting on today:**
1. DuxGroup renewal appears lapsed (Jun 30) and isn't in today's briefing — verify and escalate if real.
2. Fireblocks has no Notion registry entry — needs `/add-account` re-run or manual registry fix.
3. Papaya Global naming mismatch (curly braces vs. parens) is likely silently blocking its sync — rename either the local folder or the registry key to match.

Plus the standing structural note: 66% of the book (31/47 accounts) is >14 days stale under the current top-10-scoped harvest pattern.
