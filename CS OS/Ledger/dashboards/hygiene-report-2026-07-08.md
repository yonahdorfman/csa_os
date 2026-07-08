# Hygiene Report — 2026-07-08

Run: orchestrator tick, 10:04 GMT+3 (janitor was due 09:30, not yet run today — executed now)

## Job 1: Context Freshness (by `_context.md` "Last Updated" header)

✅ Fresh (≤7d): 16 accounts — AT&T Israel, Fibi, Forex Club, GameStory Israel, Kape Technologies, Lemonade, MyHeritage, Nanit, Nexus Mods, PayBox, PeerPlay, Solaredge, Tabnine, Taboola, boinkers.io, shortical.com

⚠️ Stale (7-14d): 2 accounts — Elementor (8d), HoneyBook (8d)

🔴 Critical (>14d): 29 accounts — Altshuler Shaham, Appcharge LTD, AutoDS IL, Base44, BlueThrone, Bookaway, ClickSoftware, Duda, DuxGroup, Ferryhopper, Fireblocks, GETT, Gamdom, Guard.io, HAAT Delivery, Loora.AI, Movement-Group, Pango Pay & Go, Papaya Global, Riverside.fm, RubyLabs, SimilarWeb, Simply, StakeMate, Stillfront Group AB, Talon Cyber Security, Whalo, mbg-digital.com, runwayer.com (17-35 days since header last bumped)

**Important caveat:** cross-checking `_MANIFEST.md` files shows almost all 47 accounts (46/47) have a dated entry from the last 7 days (as recent as today). Only Fireblocks' manifest is genuinely stale (last dated entry 2026-06-10, consistent with its "Churned" health status). This means the `_context.md` "Last Updated" header is not being consistently bumped even when the manifest/context body is actively updated — a **process/discipline issue**, not an actual data-staleness issue for most of the 29 "critical" accounts. Recommend fixing the header-update step in whatever writes `_context.md` so Job 1 reflects reality.

## Job 3: Inbox

Pending: 0 unprocessed items. Two files present (`2026-05-27-dm-notes.md`, `2026-06-30-dm-notes.md`) both already sit outside `.processed/` but contain no new content since last check — flagging for manual confirmation they were actually filed, since `.processed/` is empty.

## Job 4: Structural Hygiene

✅ 47/47 accounts have both `_context.md` and `_MANIFEST.md`.
✅ `cadence.md` and `overrides.md` present, no unfilled placeholders.
⚠️ See Job 1 caveat above — header/body drift on ~29 accounts.

## Job 5: Notion Health (spot check)

✅ Notion search responded successfully (found telemetry channel history + linked docs).
Not exhaustively spot-checked against all 47 registry entries this cycle (scope-limited) — no issues surfaced in the spot check performed.

## Job 6: Renewals & Risk Alerts

🔴 **Renewal lapsed / overdue:**
- **AT&T Israel** — lapsed 6+ days past Jun 30 expiry. SecurityScorecard review still blocking; champion (Ayelet Eyal) unresponsive. Health: Low, Risk: High.
- **GETT** — contract end 2026-05-31, now lapsed 37+ days; renewal still Stage 1: Pipeline.

🔴 **Renewal <60 days out:**
- **Elementor** — Jul 31 renewal (23 days), loss-reason review still open, risk of same objections recurring.
- **MyHeritage** — renewal ~25 days out, no formal renewal motion started yet; funnel/cohort discrepancy open 55+ days, EU data-residency deadline missed.
- **PeerPlay** — Stage 4 Negotiation, Aug 1 contract start target (~24 days), quarterly meeting on calendar today.

⚠️ **Critical health / high churn risk (independent of renewal timing):**
- **Fireblocks** — 🔴 Churned.
- **Tabnine** — Low health, actively threatening to churn over legacy B2C pricing not fitting B2B pivot.
- **shortical.com** — Low health; new website causing event-volume explosion, customer considering disabling Mixpanel for web entirely.
- **ClickSoftware** — severe risk: near-zero product usage, no named active users for 5+ weeks.
- **Gamdom** — critically low usage (223 queries/6 users), zero feature engagement despite strong onboarding.
- **Lemonade** — Low health, Negative sentiment, renewal 5+ weeks past-due with unresolved support tickets.
- **PayBox - Discount Bank** — Low health, Mixed sentiment, FY28 renewal still Stage 1.
- **Guard.io** — plan usage at 97.6% of ceiling, needs proactive tier/pricing conversation.
- **Talon Cyber Security** — acquisition by Palo Alto Networks may trigger tool consolidation/churn.

## Job 7: Communication Silence (proxy: manifest last-dated-entry)

Using `_MANIFEST.md` most recent dated entry as a proxy for last tracked activity (full contact-level Last-Engaged pass deferred — see note):

🔴 High Priority (46+ days): **Fireblocks** — last manifest activity 2026-06-10 (~28d and rising), consistent with churned status. Confirm relationship status before any further automated outreach content references it as active.

Everyone else's manifests show activity within the last 7 days — no other silence flags this cycle.

**Note:** this is a lighter-weight version of Job 7 than the full contact-level "Last Engaged" sweep across all `_MANIFEST.md` Contacts tables — that full pass was skipped this cycle for scope/time reasons and should run in a future full morning-cycle harvest.

## Summary

- **29** accounts flagged critical on file-header freshness, but this is mostly a **header-hygiene bug**, not real staleness (46/47 manifests are current).
- **2** renewals already lapsed (AT&T Israel, GETT) — both still not closed.
- **3** renewals inside 60 days needing active motion (Elementor, MyHeritage, PeerPlay).
- **9** accounts flagged Low/critical health or explicit churn risk language.
- **1** account (Fireblocks) shows genuine communication/activity silence.
