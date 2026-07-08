# Hygiene Report — 2026-07-05 (Sunday)

Run: orchestrator janitor cycle, 2026-07-05 ~11:00 GMT+3. Book size: 46 accounts.

## Job 1: Context Freshness

✅ Fresh (≤7d): 3 accounts — GameStory Israel (Jul 2), Elementor (Jun 30), HoneyBook (Jun 30)

⚠️ Stale (7-14d): 20 accounts — AT&T Israel, Altshuler Shaham, AutoDS IL, DuxGroup, Fibi - Israeli International Bank, Fireblocks, Gamdom, Guard.io, HAAT Delivery, Kape Technologies PLC (Israel), Nanit, PayBox - Discount Bank, PeerPlay, Riverside.fm, SimilarWeb, Solaredge, StakeMate, Whalo, runwayer.com, shortical.com

🔴 Critical (>14d): 23 accounts — Appcharge LTD, Base44, BlueThrone, Bookaway, ClickSoftware, Duda, Ferryhopper, Forex Club International LLC, GETT, Lemonade, Loora.AI, Movement-Group, MyHeritage Ltd. main account, Nexus Mods, Pango Pay & Go ltd, Papaya Global {Formerly Azimo}, RubyLabs, Simply, Stillfront Group AB, Tabnine (Previously Codota), Taboola, Talon Cyber Security, mbg-digital.com

**Note:** just over half the book (23/46) hasn't had a local context refresh in 2+ weeks — largest single hygiene issue this run. This is consistent with the Notion/Slack unauthenticated gap flagged in the 01:06 tick log; today's cycle is the first full harvest opportunity since then.

## Job 2: Source Validation (abbreviated)

Full per-account validation across 46 accounts x multiple sources was out of scope for a single tick given tool-call budget. Spot-checked:
- ✅ Notion search responding normally
- ✅ Slack channel #at-lemonade (CHE9ZMQM8) readable

No broken sources detected in spot-check. Full Job 2 sweep recommended as a follow-up, ideally batched via `reconcile check`.

## Job 3: Inbox Processing

Pending: 2 items (both unprocessed, dated 2026-05-27 and 2026-06-30 — well overdue for filing)

- `2026-05-27-dm-notes.md` — "Check even import errors walo" — flagged unmatched at the time, but **likely a typo/truncation for "Whalo"** (an active account in the book). Needs manual confirmation and filing.
- `2026-06-30-dm-notes.md` — "gong is set up" — informational, no account match. Corresponds to the Clari→Gong migration completing; already reflected in that day's briefing. Safe to file/archive.

## Job 4: Structural Hygiene + Drift

✅ 46/46 accounts have both `_context.md` and `_MANIFEST.md`
✅ `cadence.md` present and parseable
✅ `overrides.md` has no unfilled placeholders

Drift check (abbreviated — full reconcile not run this tick): one drift signal surfaced incidentally — **Lemonade**'s local `_context.md` is bucketed "critical" (last updated Jun 3), but its Slack channel (#at-lemonade) shows a live Weekly Account Summary bot post as recently as Jun 29 with an unresolved P1 (performance ticket), an overdue May 31 renewal, and Zendesk sentiment at a critical 1.65. **Local context is materially behind live signal for this account** — recommend Lemonade be first in line for today's harvest/compress pass.

## Job 5: Notion Health

✅ Notion workspace search responding. Spot-check of 2-3 Context pages and full registry stale-entry check not run this tick (abbreviated for budget) — recommend as follow-up.

## Job 6: Upcoming Renewals + Risk Flags

🔴 **Renewal <60d:**
- Guard.io — Stage 4: Negotiation, $81,469, close date ~5 days out
- Elementor — Jul 31 renewal conversation pending; risk of churn if same objections resurface
- Lemonade — renewal was **May 31, now overdue**, no confirmed close as of Jun 29

⚠️ **Critical health / churn risk:**
- Fireblocks — marked 🔴 Churned; stub account, no BQ/usage data ever pulled
- shortical.com — CRITICAL: new site caused event-volume explosion, pricing unsustainable, customer considering dropping Mixpanel for web entirely
- Tabnine — customer actively threatening churn over legacy B2C pricing structure vs. B2B usage
- ClickSoftware — severe risk: near-zero usage (51 unattributed queries/week), no named active users in 5+ weeks, FY27 renewal in pipeline
- Gamdom — critically low usage (223 queries, 6 users) and zero feature engagement despite a strong onboarding call
- Talon Cyber Security — Palo Alto Networks acquisition raises tool-consolidation/churn risk
- boinkers.io — single-threaded relationship; sole power user hit 4 separate bugs in a week, frustration/churn risk
- Taboola — "basic" A/B-testing-only usage, Deeper Dive pitch stalled, risk flagged for Mar 2027 renewal (not urgent, but worth tracking)

⚠️ **Overdue items:** Lemonade — Jun 16 performance ticket still unconfirmed resolved as of Jun 29 (19+ days).

## Summary

**26 warnings surfaced this run** (23 critical-freshness accounts, 2 unprocessed inbox items, 1 drift signal — renewal/health flags counted separately in Job 6: 3 renewals <60d, 8 churn-risk accounts, 1 overdue item).

Worst single issue: **Lemonade** — overdue renewal + unresolved P1 + stale local context, all at once.

---

### Addendum — 16:xx GMT+3 orchestrator tick (same day)

This report already existed when the 16:xx tick ran (evidence of an earlier, unlogged janitor execution today — the changelog only shows a `01:06` entry with no `scheduler:completed:janitor` marker, so this run is backfilling that gap rather than re-running Job 1–6 from scratch).

Cross-check against a fresh BQ pull (`account_metadata` + `account_plans_current`) run at 16:xx:
- **Lemonade renewal:** BQ shows the FY27 renewal **closed won in May** (not overdue as this report's Job 6 states from stale local context) — FY28 Renewal is now in early pipeline. The account is still genuinely critical on health (`tldr_health_rating: Low`, `sentiment_label: Negative`) — unresolved `.set` tracking/identity bug, an active performance/slowness issue, 3 NPS detractors, and a contact transition (Hadas Lustgarten → Diana Chausskiy). Recommend treating "renewal" language above as superseded by this fresher read, but keep Lemonade as a First Things First item on health grounds alone.
- **AT&T Israel** (not covered by this account's CSA-matched BQ pull in this session — carried from the 2026-07-02 EoD retro instead): renewal expired Jun 30, still unresolved as of the last logged update. No fresher signal obtained this tick.
- **GETT:** BQ confirms `commercials_contract_end` 2026-05-31 already lapsed, deal still Stage 1: Pipeline, no verbal commitment — consistent with this report's concern, now with an added signal: NPS dropped 9→8 and Lyft's acquisition of GETT's UK business is adding organizational uncertainty.
- **New from BQ, not in the original Job 6 list:** Tabnine's champion (Ory Yassur) is now *actively* threatening churn over an MTU pricing dispute (auto-renewal stopped); shortical.com's quota overage has hit **116.99%** with the customer weighing disabling Mixpanel on web entirely; PayBox has an active **$256K downgrade** opportunity in Stage 4 tied to parent-bank (Discount Bank) layoffs; ClickSoftware remains at zero adoption 10+ weeks post-renewal.

Backfilling changelog now with `scheduler:completed:janitor` (reflecting this addendum, not a full Job 1-6 re-run) so future ticks don't re-trigger janitor today.
