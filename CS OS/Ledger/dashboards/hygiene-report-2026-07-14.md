# Hygiene Report — 2026-07-14

Run as the 9:30am weekdays janitor cadence (on time, ~10:05am tick — first tick after 9:30am threshold). Morning-cycle already completed on time at 09:04. Consistent with the last several cycles, Jobs 2/4/5/7 are spot-checked rather than run exhaustively across the full book, given its size — flagged explicitly below rather than silently skipped.

## Job 1: Context Freshness (47 local account folders)

✅ Fresh (≤7d): 10 — Fibi - Israeli International Bank, Fireblocks, Forex Club International LLC, Kape Technologies PLC (Israel), Lemonade, Nexus Mods, PayBox - Discount Bank, Solaredge, Taboola, boinkers.io BY Acid Labs

⚠️ Stale (7-14d): 9 — AT&T Israel (07-06, 8d), MyHeritage Ltd. main account (07-06, 8d), Nanit (07-06, 8d), PeerPlay (07-06, 8d), Tabnine (07-06, 8d), shortical.com (07-06, 8d), Elementor (06-30, 14d), GameStory Israel (07-02, 12d), HoneyBook (06-30, 14d)

🔴 Critical (>14d): 28 — Altshuler Shaham, Appcharge LTD, AutoDS IL, Base44, BlueThrone, Bookaway, ClickSoftware, Duda, DuxGroup, Ferryhopper, GETT, Gamdom, Guard.io, HAAT Delivery, Loora.AI, Movement-Group, Pango Pay & Go ltd, Papaya Global {Formerly Azimo}, Riverside.fm, RubyLabs, SimilarWeb, Simply, StakeMate, Stillfront Group AB, Talon Cyber Security, Whalo, mbg-digital.com, runwayer.com

Note: six accounts crossed from fresh→stale since yesterday's report simply by the calendar turning over a day (AT&T Israel, MyHeritage, Nanit, PeerPlay, Tabnine, shortical.com — all last touched 07-06). This is the local `_context.md` freshness proxy only; no new harvesting happened for these six today beyond this morning's Gmail/Calendar-scoped morning-cycle.

## Job 2: Source Validation

Spot-checked only (Lemonade, MyHeritage, Nexus Mods — all had Gmail/Calendar activity in this morning's harvest and resolved fine). Full 47-account source validation not run this cycle — same scope call as recent cycles.

## Job 3: Inbox Processing

2 items in `Inbox/`, both already classified in-place (not moved to `.processed/`):
- `2026-05-27-dm-notes.md` — processed prior cycle
- `2026-06-30-dm-notes.md` — "gong is set up" note, unmatched (infra, not account-specific), already annotated

No new unprocessed inbox items since yesterday.

## Job 4: Structural Hygiene + Drift

Not run in full this cycle (same scope call as Job 1/2). Known carryover: **Fireblocks** remains an orphaned account (no Notion registry entry, stub context file, no Slack/BQ data) — flagged in the last several hygiene reports with no resolution yet. This is now a multi-week open item worth a direct decision (either register it properly in Notion or explicitly retire it from the book).

## Job 5: Notion Health

Not independently re-verified this cycle (this morning's Notion write for the briefing child page succeeded, treated as evidence the connector is healthy).

## Job 6: Renewals & Risk Alerts

🔴 **Past-due renewals (already lapsed):**
- **AT&T Israel** — renewal date 2026-06-30, now 14 days past. Opp: FY27 Renewal + Metric Trees, Stage 4 Negotiation, $63,205. Health Low / Risk High.
- **Lemonade** — renewal date 2026-05-31, now 44 days past. Opp: FY28 Renewal, Stage 1 Pipeline, $80,284. Health Low / Risk High / Sentiment Negative. Note: this morning's Gmail harvest flagged an Asana digest snippet suggesting MyHeritage (not Lemonade) may have moved to Closed Won — unconfirmed, needs SFDC check, do not treat as fact yet. Lemonade's own renewal-lapsed status is unrelated and still unresolved.

🔴 **Renewal <60d (upcoming):**
- **Elementor** — 2026-07-31 (17 days out)
- **MyHeritage Ltd. main account** — 2026-07-31 (17 days out). Opp Stage 1 Pipeline, $110,000, Health Low / Risk High. See the unconfirmed Closed-Won signal above — worth a direct SFDC check before next cycle given it directly affects this renewal date's status.
- **GameStory Israel** — 2026-08-01 (18 days out)
- **Whalo** — 2026-08-31 (48 days out)

⚠️ **Other high-risk accounts (Health Low/Risk High from last BQ refresh):** PayBox - Discount Bank ($128,299, Stage 1 Pipeline, downgrade risk noted), Tabnine ($93,073, Stage 1 Pipeline), shortical.com ($76,007, Stage 1 Pipeline), PeerPlay (Health Medium/Risk High, Stage 4 Negotiation, $90,999 — closer to close but still flagged).

Overdue open items (sampled, not recomputed across all 47 accounts this cycle): Lemonade carries 4 open items past due (Jul 6-9) — renewal commercial discussion, Zendesk ticket #699316, `.set` tracking bug, NPS detractor follow-up.

## Job 7: Communication Silence

Not recomputed in full this cycle (would require per-account Recent History / contact Last-Engaged review across 47 accounts) — same scope call as the last several cycles. This is now the third consecutive janitor run without a real Job 7 number; today's report should not be read as "0 flagged," it's simply not computed. Recommend Yonah trigger a dedicated `/reconcile check` or full janitor pass if a real silence read is wanted — this has been flagged in each of the last three hygiene reports without action.

Exempt (untracked channel, per `resources/silence-exempt-accounts.md`): AT&T Israel (WhatsApp) — flagged here as a periodic nudge to confirm the relationship is still active via that channel, not as a risk.

## Summary

**Warnings: 6 flagged categories** — freshness (28 critical / 9 stale), 2 lapsed renewals, 4 upcoming renewals <60d, orphaned Fireblocks account (unresolved for weeks), unquantified overdue items, unquantified Job 7 silence (3rd cycle running uncomputed).

Recommend a full `/reconcile check` + full janitor pass soon — it's been roughly a month since the last thorough freshness/drift/silence sweep, and the same scope-call caveats are recurring cycle after cycle rather than being resolved.
