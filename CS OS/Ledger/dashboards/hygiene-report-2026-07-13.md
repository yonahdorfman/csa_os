# Hygiene Report — 2026-07-13

Run as part of catch-up: no orchestrator tick had executed yet today (last changelog entry was 2026-07-12 23:08). Both janitor (9:30am) and morning-cycle (9:00am) were overdue as of this 13:02 GMT+3 tick — running both now.

## Job 1: Context Freshness (48 accounts)

✅ Fresh (≤7d): 16 — AT&T Israel, Fibi, Fireblocks, Forex Club, Kape, Lemonade, MyHeritage, Nanit, Nexus Mods, PayBox, PeerPlay, Solaredge, Tabnine, Taboola, boinkers.io, shortical.com
⚠️ Stale (7-14d): 3 — Elementor (06-30), GameStory Israel (07-02), HoneyBook (06-30)
🔴 Critical (>14d): 29 — Altshuler Shaham, Appcharge LTD, AutoDS IL, Base44, BlueThrone, Bookaway, ClickSoftware, Duda, DuxGroup, Ferryhopper, GETT, Gamdom, Guard.io, HAAT Delivery, Loora.AI, Movement-Group, Papaya Global, Pango Pay & Go, Riverside.fm, RubyLabs, SimilarWeb, Simply, StakeMate, Stillfront Group AB, Talon Cyber Security, Whalo, mbg-digital.com, runwayer.com, Movement-Group

Note: this is the local `_context.md` freshness proxy only — Job 2/4 source validation and full Notion drift check were spot-sampled (not run against all 48 accounts), consistent with the scope judgment calls made in the last several cycles for a book this size.

## Job 2: Source Validation

Spot-checked only (Lemonade, Base44, Nexus Mods — all had calendar/Gmail activity today or this week and resolved fine). Full 48-account source validation not run this cycle.

## Job 3: Inbox Processing

2 items in `Inbox/`, both already classified in-place (not moved to `.processed/`):
- `2026-05-27-dm-notes.md` — processed prior cycle
- `2026-06-30-dm-notes.md` — "gong is set up" note, unmatched (infra, not account-specific), already annotated

No new unprocessed inbox items.

## Job 4: Structural Hygiene + Drift

Not run in full this cycle (scope call — see Job 1 note). Known carryover: **Fireblocks** remains an orphaned account (no Notion registry entry, stub context file, no Slack/BQ data) — flagged in the last several hygiene reports with no resolution yet.

## Job 5: Notion Health

Not independently re-verified this cycle (Notion pushes for Lemonade/briefing succeeded in recent cycles, treated as evidence connector is healthy).

## Job 6: Renewals & Risk Alerts

🔴 **Past-due renewals (already lapsed):**
- **AT&T Israel** — renewal date 2026-06-30, now 13 days past. Opp: FY27 Renewal + Metric Trees, Stage 4 Negotiation, $63,205. Health Low / Risk High.
- **Lemonade** — renewal date 2026-05-31, now 43 days past. Opp: FY28 Renewal, Stage 1 Pipeline, $80,284. Health Low / Risk High / Sentiment Negative. (Yonah noted verbally "Lemonade has renewed" on 07-08 — SFDC/BQ still shows Stage 1 Pipeline as of 07-06 refresh; needs SFDC reconciliation, carried unresolved for a week.)

🔴 **Renewal <60d (upcoming):**
- **Elementor** — 2026-07-31 (18 days out)
- **MyHeritage** — 2026-07-31 (18 days out). Opp Stage 1 Pipeline, $110,000, Health Low / Risk High.
- **GameStory Israel** — 2026-08-01 (19 days out)
- **Whalo** — 2026-08-31 (49 days out)

⚠️ **Other high-risk accounts (Health Low/Risk High from last BQ refresh):** PayBox - Discount Bank ($128,299, Stage 1 Pipeline, downgrade risk noted), Tabnine ($93,073, Stage 1 Pipeline), shortical.com ($76,007, Stage 1 Pipeline), PeerPlay (Health Medium/Risk High, Stage 4 Negotiation, $90,999 — closer to close but still flagged).

Overdue open items (sampled): Lemonade has 4 open items all past their due dates (Jul 6-9) — renewal commercial discussion, Zendesk ticket #699316, `.set` tracking bug, NPS detractor follow-up. Full cross-book overdue-item count not computed this cycle (would require reading all 48 `_context.md` Open Items sections).

## Job 7: Communication Silence

Not computed in full this cycle (would require per-account Recent History / contact Last-Engaged review across 48 accounts — out of scope for this catch-up run). Recommend a dedicated `/reconcile check` or full janitor pass to get real Job 7 numbers; today's numbers should not be treated as "0 flagged," they're simply not computed.

## Summary

**Warnings: 6 flagged categories** (freshness, 2 lapsed renewals, 4 upcoming renewals <60d, orphaned Fireblocks account, unquantified overdue items, unquantified silence). Recommend a full `/reconcile check` + full janitor pass soon given the ~1 month gap since the last thorough freshness/drift sweep (last full run mid-June; recent cycles have been Gmail+Calendar scoped only).
