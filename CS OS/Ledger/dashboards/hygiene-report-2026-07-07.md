# Hygiene Report — 2026-07-07 (Tuesday)

Run: 11:12 GMT+3 (delayed — due 09:30, orchestrator tick at 08:50 had not yet detected it as due; caught up this tick)

## Job 1: Context Freshness

✅ Fresh (≤7d): 11 accounts — AT&T Israel, Elementor, GameStory Israel, HoneyBook, Lemonade, MyHeritage Ltd. main account, Nanit, PayBox - Discount Bank, PeerPlay, Tabnine (Previously Codota), shortical.com
⚠️ Stale (7-14d): 0 accounts
🔴 Critical (>14d): 35 accounts — Altshuler Shaham, Appcharge LTD, AutoDS IL, Base44, BlueThrone, Bookaway, ClickSoftware, Duda, DuxGroup, Ferryhopper, Fibi - Israeli International Bank, Fireblocks, Forex Club International LLC, GETT, Gamdom, Guard.io, HAAT Delivery, Kape Technologies PLC (Israel), Loora.AI, Movement-Group, Nexus Mods, Pango Pay & Go ltd, Papaya Global {Formerly Azimo}, Riverside.fm, RubyLabs, SimilarWeb, Simply, Solaredge, StakeMate, Stillfront Group AB, Talon Cyber Security, Whalo, boinkers.io BY Acid Labs, mbg-digital.com, runwayer.com

Note: the book is bimodal — accounts either touched in the last week or untouched for 15+ days, with nothing in between. Freshness is driven by whether compress found an actual signal, not a system failure (e.g. today's compress cycle found zero actionable patches). Worth a manual sanity check on whether the 35 critical accounts are genuinely quiet or whether harvesters are missing signal for them (Slack full-book, Drive, and Notion-content harvesters have been running in `skipped:budget` mode for multiple days — see Harvest note below).

## Job 2: Source Validation (spot check)

Spot-checked 2 of 46 accounts (AT&T Israel, Lemonade) given full 46-account validation was out of scope for a single tick:
✅ Notion Context pages: 2/2 accessible
✅ Slack channels: 2/2 accessible (#at-and-t-global, #at-lemonade)

No broken sources found in the sample. Full validation not run this cycle.

## Job 3: Inbox

Pending: 2 items sitting in `Inbox/` root, neither moved to `.processed/` (folder is empty):
- `2026-05-27-dm-notes.md` — unmatched ("walo" — likely truncated "Whalo"; still unresolved 41 days later)
- `2026-06-30-dm-notes.md` — Gong setup note, already actioned per changelog (referenced in 07-01 briefing) but never archived

⚠️ Structural gap: the Inbox → `.processed/` archiving step doesn't appear to be running even when items are actioned. Recommend a `/sync` or manual pass to move handled items out of the root.

## Job 4: Structural Hygiene + Drift

✅ 46/47 account folders have complete `_context.md` + `_MANIFEST.md` + registry entry
⚠️ **Missing registry entry:** Fireblocks — folder exists locally with full `_context.md`/`_MANIFEST.md`, but no `ACCOUNT:Fireblocks=...` entry in `resources/notion-page-registry.md`. This account is likely not syncing to Notion at all.
⚠️ **Naming drift:** local folder is `Papaya Global {Formerly Azimo}` (curly braces) but registry entry is `Papaya Global (Formerly Azimo)` (parentheses) — same account, mismatched key. Could cause silent sync misses if any lookup does exact string matching.

Drift check (reconcile) not run in full this cycle — recommend running `/reconcile check` explicitly given the Fireblocks and Papaya Global findings above.

## Job 5: Notion Health

✅ Accounts DB responsive
✅ 2/2 spot-checked Context pages rendered correctly (AT&T Israel, Lemonade)
⚠️ Fireblocks has no page to check — see Job 4

## Job 6: Alerts

🔴 **Renewal overdue (lapsed, unresolved):**
- AT&T Israel — $63,205, expired Jun 30, now 7 days lapsed. Ayelet Eyal unresponsive since Jun 15. SecurityScorecard review still blocking. Escalation path was miscast as of yesterday's DM correction — needs re-scoping with the actual AT&T decision maker.
- Lemonade — $80,284, past due since May 31 (5+ weeks). Jul 6 QBR held but outcome not yet confirmed by any harvester.

🔴 **Renewal <60 days:**
- Elementor — Jul 31 (24 days)
- MyHeritage Ltd. main account — ~25 days out, no formal renewal motion started yet; EU data-residency deadline also missed and a 55+ day funnel/cohort discrepancy is still open

⚠️ **Critical health (Low) / High risk:**
- AT&T Israel, Lemonade, MyHeritage, PayBox - Discount Bank, Tabnine (Previously Codota), shortical.com

⚠️ **Overdue open items (>7 days):**
- MyHeritage funnel/cohort discrepancy — open 55+ days
- StakeMate SR enterprise trial approval — requested Jun 3, still pending Ofer Polivoda sign-off (34 days)

## Summary

**43 warnings this cycle:** 35 critical-freshness, 2 renewals overdue, 2 renewals <60d, 1 missing Notion registry entry, 1 naming drift, 2 unarchived Inbox items.

Top priority: AT&T Israel and Lemonade are both past-due renewals sitting in the same "unresolved for a full week+" pattern with no confirmed next step from Tal Huberman on either. Both need a direct check-in today, not another passive wait.
