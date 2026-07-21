# Hygiene Report — 2026-07-12

Book size: 47 account folders under `Accounts/`.

## Job 1: Context Freshness

✅ Fresh (≤7d): 16 accounts — AT&T Israel, Fibi, Fireblocks, Forex Club International LLC, Kape Technologies, Lemonade, MyHeritage, Nanit, Nexus Mods, PayBox - Discount Bank, PeerPlay, Solaredge, Tabnine, Taboola, boinkers.io BY Acid Labs, shortical.com
⚠️ Stale (7-14d): 3 — Elementor (12d), GameStory Israel (10d), HoneyBook (12d)
🔴 Critical (>14d): 28 — Altshuler Shaham (21d), Appcharge LTD (39d), AutoDS IL (21d), Base44 (39d), BlueThrone (39d), Bookaway (32d), ClickSoftware (39d), Duda (39d), DuxGroup (20d), Ferryhopper (32d), GETT (31d), Gamdom (20d), Guard.io (20d), HAAT Delivery (21d), Loora.AI (32d), Movement-Group (39d), Pango Pay & Go ltd (32d), Papaya Global (39d), Riverside.fm (21d), RubyLabs (32d), SimilarWeb (21d), Simply (32d), StakeMate (21d), Stillfront Group AB (32d), Talon Cyber Security (39d), Whalo (21d), mbg-digital.com (39d), runwayer.com (21d)

## Job 2: Source Validation

Spot-checked (full 47-account validation not run this cycle — sampled for signal):
✅ Slack #at-lemonade (CHE9ZMQM8) — accessible, recent traffic (weekly summary bot post Jul 5)
✅ Slack #similarweb-mixpanel (C03A3S5GB47) — accessible (channel quiet since Jun 23 membership-change event, no error)
✅ Notion Context page — Lemonade — accessible, rich body, in sync with local
✅ Notion parent page — Nanit — accessible, properties readable (Health: Medium, Renewal: 2027-08-01)

No broken sources detected in the sample. Full validation of all 47 Slack channels / Notion pages was not run this cycle (out of scope for a signal-based cycle) — recommend a dedicated `/janitor deep` pass if broken-source risk is a concern.

## Job 3: Inbox

Pending: 2 items in `Inbox/`
- `2026-05-27-dm-notes.md` — unprocessed, age ~46d
- `2026-06-30-dm-notes.md` — unprocessed, age ~12d

Both are unmatched/unfiled DM notes — needs manual triage (not auto-matched by filename to a single account).

## Job 4: Structure + Sync

✅ 47/47 account folders have `_context.md` present.
⚠️ Orphaned folder / missing registry entry: **Fireblocks** — has a local account folder and a Slack channel registry entry (`#at-fireblocks`), but **no entry in `resources/notion-page-registry.md`**. Its `_context.md` confirms it's a stub ("No Slack channels configured... run a BQ pull to retrieve ARR, renewal date..." — note this text is stale/wrong, since Slack channels for Fireblocks DO exist in the registry). This account needs `/add-account` or a manual registry fix to get a real Notion page.

Drift check: Sampled Lemonade and Nanit — both in sync (local `_context.md` dates match Notion `Last Updated` properties). Full 47-account reconcile not run this cycle.

## Job 5: Notion Health

✅ Accounts DB accessible (`cfe01032176e455eb849315deff4a68c`)
✅ 2/2 spot-checked pages OK (Lemonade Context, Nanit parent)
⚠️ Stale registry entries: none detected in sample; Fireblocks is *missing* (not stale) — see Job 4.

## Job 6: Upcoming Renewals + Risk Flags

🔴 Renewal <60d (by 2026-09-10):
- **MyHeritage** — FY27 Renewal, $110,000, closing 2026-08-01 (20 days out) — Health: Low, Risk: High
- **GameStory Israel** — $50,400 renewal opportunity closing 2026-08-01 (20 days out)
- **HoneyBook** — FY27 Renewal, $61,152, "July 2026 renewal approaches" — competitive threat from Statsig live
- **Elementor** — FY27 Renewal, $117,200 + $35K upsell, "before the July renewal" — consumption mismatch (800K actual vs 1.5M projected MTUs) unresolved
- **Lemonade** — FY28 Renewal, $80,284 — already **5+ weeks past due**, still in Stage 1: Pipeline per BQ (see also DuxGroup note below re: a similar false-positive pattern)

⚠️ Critical health (Health: Low, Risk: High in Sales Intelligence): **AT&T Israel, Lemonade, MyHeritage, PayBox - Discount Bank, Tabnine, shortical.com** (6 accounts) + **PeerPlay** (Health: Medium, Risk: High)

⚠️ Overdue items (7+ days late, non-exhaustive — items found via full-book grep): 40+ overdue open items across AT&T Israel, Altshuler Shaham, Duda, Elementor (EU data residency, now resolved per note), Ferryhopper, HAAT Delivery, HoneyBook (6 overdue items — worst in book), Lemonade, MyHeritage, Nexus Mods, PeerPlay, RubyLabs, SimilarWeb, StakeMate, Stillfront Group AB (one overdue since **Apr 6** — oldest in book). Recurring theme: **EU data residency** open items overdue since Jul 1 across Duda, Elementor, MyHeritage, RubyLabs, SimilarWeb — looks like a systemic/product-level issue worth a cross-account escalation rather than five separate follow-ups.

Note: DuxGroup's Notion "Renewal Date" property shows a likely false-positive lapsed-renewal flag (property-sync gap vs. the account's own Closed Won note) — same pattern flagged on PayBox. Recommend a `/reconcile fix` pass on Notion DB renewal-date properties.

## Job 7: Communication Silence Check

Methodology note: full Manifest "Last Engaged" aggregation across 47 accounts was out of scope this cycle; used the most recent past-dated signal found anywhere in each account's `_context.md` as a proxy for last tracked contact.

🔴 High Priority Outreach (46+ days silent): 0 accounts

⚠️ Needs Outreach (22-45 days silent): 10 accounts
- **Papaya Global** — 39d — try: re-engage on Fine.dev acquisition / Uprise partnership news as a business-momentum opener
- **Talon Cyber Security** — 39d — try: follow up on the tentative lunch with Adi Azriel — flagged as the make-or-break signal on renewal intent given the Palo Alto Networks consolidation risk
- **ClickSoftware** — 32d — try: activation outreach ahead of FY27 renewal — near-zero usage (51 unattributed queries) and no named active user in 5+ weeks is a churn setup
- **Ferryhopper** — 32d — try: close out the 3 overdue open items (nested-properties consult, account-strategy call with Max, follow-up meeting) before the FY29 renewal conversation drifts further
- **Loora.AI** — 32d — try: check in ahead of FY-cycle renewal pipeline movement (no fresh signal since Jun 10)
- **Movement-Group** — 32d — try: close the loop on the myIQ / PII data-flow blocker — last touch was about this exact sensitivity
- **Simply** — 27d — try: confirm the June 3 Quarterly Meeting actually happened and capture outcome — deal is "FY28 Early Renewal + Upsell" at $190K, too big to let go quiet
- **Stillfront Group AB** — 31d — try: close two long-overdue items — confirm warehouse access/contractual status with AE Alexander Padberg, and reply to Nils Lobe's Apr 6 DM (now 3+ months unanswered)
- **Appcharge LTD** — 27d — try: check status of the pending Warehouse Ingestion (WHC) enterprise trial approval — good-health account, don't let a live trial go stale
- **mbg-digital.com** — 32d — try: resolve the open billing/payment-verification issue that's blocking a smooth onboarding

Exempt — Verify Manually (untracked primary channel): 1 account
- **AT&T Israel** (WhatsApp) — 4d since last tracked contact — confirm the relationship is still active via WhatsApp; tracked channel looks fine but that's not the primary channel

## Summary

- **47 accounts** in book. **28 critical / 3 stale** on file freshness.
- **1 orphaned account** (Fireblocks — no Notion registry entry).
- **2 unprocessed Inbox items** needing manual triage.
- **5 renewals inside 60 days**, one of them (Lemonade) already 5+ weeks overdue.
- **7 accounts at critical health** (Low/Medium health + High risk).
- **10 accounts** in the "Needs Outreach" silence tier, **0** in "High Priority."
- **Systemic issue flagged:** EU data-residency routing overdue on 5 separate accounts — recommend escalating as one engineering/product ask instead of five individual follow-ups.
