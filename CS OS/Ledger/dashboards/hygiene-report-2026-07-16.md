# Hygiene Report — 2026-07-16

Run: Janitor (weekday 9:30am cadence), executed as part of today's orchestrator tick.
Book size: 46 local accounts under `Accounts/`.

---

### Job 1: Context Freshness

✅ Fresh (≤7d): 8 accounts — Base44, Kape Technologies PLC (Israel), Lemonade, MyHeritage Ltd. main account, Nexus Mods, RubyLabs, Stillfront Group AB, Whalo
⚠️ Stale (7-14d): 12 accounts — AT&T Israel, Fibi - Israeli International Bank, Forex Club International LLC, GameStory Israel, Nanit, PayBox - Discount Bank, PeerPlay, Solaredge, Tabnine (Previously Codota), Taboola, boinkers.io BY Acid Labs, shortical.com
🔴 Critical (>14d): 26 accounts — Altshuler Shaham, Appcharge LTD, AutoDS IL, BlueThrone, Bookaway, ClickSoftware, Duda, DuxGroup, Elementor, Ferryhopper, GETT (35d — see note below, refreshed this cycle), Gamdom, Guard.io, HAAT Delivery, HoneyBook, Loora.AI, Movement-Group, Pango Pay & Go ltd, Papaya Global {Formerly Azimo}, Riverside.fm, SimilarWeb, Simply, StakeMate, Talon Cyber Security, mbg-digital.com, runwayer.com

**Note:** GETT's local `_context.md` was found 35 days stale (Last Updated 2026-06-11) despite active Slack engagement through mid-July (Jul 9 onsite, Jul 12 SR-upsell signal, new ticket #10057). This is a genuine sync gap, not just quiet-account staleness — the morning harvest caught and patched it this cycle (see changelog `compress:wrote:GETT`). Snapshot above reflects state at janitor run time, before that patch.

---

### Job 2: Source Validation

✅ Notion connectivity confirmed (self-fetch OK, workspace: Mixpanel). GETT Context child page fetched and verified — ancestor path matches "GETT", body was well-populated (not a stub).
✅ Slack connectivity confirmed — 19 priority-account internal channels read successfully this cycle (see harvest section of changelog).
⚠️ Full source validation (all 46 accounts × all linked sources) was not run exhaustively this cycle due to tool-call budget — scoped to the accounts touched by this cycle's harvest (see Notes). No broken sources detected among those checked.

---

### Job 3: Inbox

Pending: 2 items (both pre-existing, not new today)
• 2026-05-27-dm-notes.md — unprocessed
• 2026-06-30-dm-notes.md — unprocessed

---

### Job 4: Structure + Sync

✅ 46/46 local accounts have both `_context.md` and `_MANIFEST.md`.
✅ `cadence.md` present and parseable. `overrides.md` has no unfilled placeholders.
⚠️ **4 accounts appear in BigQuery `account_metadata` matched to CSA "Yonah" but have no local folder:** Island, Intel (Previously Screenovate), Wilma Partners Limited, Fireblocks. Not actioned this cycle (no `/add-account` run, per scope) — flagging for manual triage: confirm whether these are new/shared accounts that need onboarding or a CSA-name mismatch artifact.

Drift check: not run as a full `/reconcile check` this cycle (scoped out for budget); GETT drift (local 35d-stale vs. active Slack) was caught and corrected ad hoc during harvest.

---

### Job 5: Notion

✅ Accounts DB / workspace accessible (self-fetch confirmed, user: Yonah Dorfman).
✅ 1/1 spot-checked page OK (GETT Context page — body populated, ancestor path verified).
No stale registry entries detected in the accounts touched this cycle.

---

### Job 6: Alerts

🔴 Renewal lapsed: **GETT** — Contract End 2026-05-31, now 46 days lapsed, renewal opp still Stage 1: Pipeline.
🔴 Mixpanel plan overage: **shortical.com** at 140% of plan (known churn-risk issue, already tracked).
⚠️ Approaching plan limit (>80%): **PeerPlay** (96.3%), **Nanit** (95.8%), **Loora.AI** (85.9%), **HoneyBook** (80.6%).
⚠️ Critical health / High renewal risk (12): Altshuler Shaham, ClickSoftware, Ferryhopper, Fibi - Israeli International Bank, Gamdom, Movement-Group, PayBox - Discount Bank, PeerPlay, Tabnine (Previously Codota), Taboola, Talon Cyber Security, and GETT (lapsed, effectively High).
⚠️ Overdue items: 26 accounts have at least one item flagged OVERDUE in `_context.md`; ~90+ individual overdue line items across the book (heaviest concentrations: AT&T Israel, Altshuler Shaham, HoneyBook, PeerPlay, Nexus Mods, RubyLabs — EU data-residency follow-ups still open on 7 accounts: Duda, Elementor, MyHeritage, RubyLabs, SimilarWeb, boinkers.io, and one more per Recent History cross-references).

---

### Job 7: Communication Silence

🔴 High Priority Outreach (46+ days silent): 0 accounts this cycle (max observed gap: 43 days).

⚠️ Needs Outreach (22-45 days silent): 23 accounts
• Altshuler Shaham — 25d since last tracked contact (2026-06-21) — try: FY28 renewal ($53,899, Stage 1) has momentum after account turned "clean" health status June 29; open the commercial conversation now while relationship energy from the Jun 16 hands-on session is still warm.
• Appcharge LTD — 43d (file) / but Slack shows active weekly engagement through Jul 12 — try: SR enterprise trial approval has been pending Ofer Polivoda since Jul 2; close that loop.
• AutoDS IL — 25d (2026-06-21) — try: revive the enterprise trial conversion / $20K upsell evaluation that stalled mid-June.
• BlueThrone — 43d (2026-06-03) — try: confirm which of the two WHC enterprise-trial requests (filed same day, different end dates) is actually live.
• Bookaway — 36d (2026-06-10) — try: customer asked to recover a deleted board 15 days before that update and it went unreplied; close that out as a low-effort trust-builder.
• ClickSoftware — 43d (2026-06-03) — try: zero named-user usage 13+ weeks post-renewal with one unresponsive contact (Vince Valentino); this is a churn-risk account that needs escalated outreach, not a routine touch.
• Duda — 43d (2026-06-03) — try: EU data-residency fix for the US-routed project was flagged Jun 21/24 with no confirmation — close the loop before it becomes a compliance escalation.
• DuxGroup — 24d (2026-06-22) — try: confirm the Notion "Renewal Date" property drift (showing lapsed when the FY27 deal actually closed Jun 20) is fixed via `/reconcile fix`, and use the outreach to confirm the close landed well with the customer.
• Ferryhopper — 36d (2026-06-10) — try: FY29 renewal ($52K, Stage 1) has no motion; combine with the still-open nested-properties follow-up from the May 25 call.
• GETT — 35d by file (now refreshed this cycle) — try: past-due renewal + the fresh SR-upsell signal from Jul 12 give a natural expansion-plus-renewal reason to reach out this week.
• Gamdom — 24d (2026-06-22) — try: 7 UCR action items from mid-June (event descriptions, AI/MCP enablement) are unconfirmed on a $103K account with critically low query volume — this is the account to prioritize.
• Guard.io — 24d (2026-06-22) — try: account has sat at 97-101% of plan ceiling for multiple weeks; the overage/tier conversation with Shavit is still unresolved post-renewal.
• HAAT Delivery — 25d (2026-06-21) — try: BigQuery integration follow-up and an unassigned Session Replay alert are both still open.
• Loora.AI — 36d (2026-06-10) — try: account is at 85.9% of plan; get ahead of the overage conversation before it becomes urgent.
• Movement-Group — 43d (2026-06-03) — try: single named user, near-zero feature adoption; Slack shows it was independently flagged this week too — a proactive check-in is overdue on both fronts.
• Pango Pay & Go ltd — 36d (2026-06-10) — try: a May 26 funnel/identity debugging thread never got a confirmed resolution — check whether the customer actually got what they needed.
• Papaya Global {Formerly Azimo} — 43d (2026-06-03) — try: account has never had a real touchpoint since BQ seeding (no contacts on file) — worth a first genuine outreach to establish the relationship.
• Riverside.fm — 25d (2026-06-21) — try: Session Replay activation has been a stated goal with no progress; use it as the outreach hook.
• SimilarWeb — 25d (2026-06-21) — try: rescheduled meeting with Ishai Gal On from early June never got rebooked.
• Simply — 36d (2026-06-10) — try: strong usage (22K+ queries, active MCP adoption) but the SR UI walkthrough with Roman has been overdue since May 27 — pair outreach with the FY28 $190K early-renewal conversation.
• StakeMate — 25d (2026-06-21) — try: SR trial approval request to Ofer has had no confirmation since Jun 3.
• Talon Cyber Security — 43d (2026-06-03) — try: usage keeps declining post-Palo-Alto-acquisition with no meeting booked; renewal intent (Oct 31) still undetermined — this needs an exec-to-exec push, not a routine check-in.
• mbg-digital.com — 43d (2026-06-03) — try: billing/payment verification issue is still open, and several purchased features (SR, Feature Flags, Experiments) remain unactivated.
• runwayer.com — 25d (2026-06-21) — try: a "Project Stopped Sending Data" alert has been unassigned and unresolved since May 29.

Exempt — Verify Manually (untracked primary channel): 1 account
• AT&T Israel (WhatsApp) — 10d since last tracked (Gmail/Slack/Calendar) contact — confirm the WhatsApp relationship is still active; note the renewal-related Open Items on this account are separately overdue and worth checking regardless of channel.

---

## Summary

- **Warnings found:** Yes (26 critical-freshness accounts, 1 lapsed renewal, 5 plan-usage alerts, 12 high-renewal-risk accounts, 26 accounts with overdue items, 23 needing outreach, 4 unmapped BQ accounts). Per janitor.md, this warrants a Slack DM post.
- **Scope note:** Given the size of the book (46 accounts) and this cycle's tool budget, Jobs 2/4's cross-source validation and `/reconcile check` were scoped to spot-checks rather than exhaustive per-account runs. This is a judgment call, logged here and in the changelog, not a silent skip.
