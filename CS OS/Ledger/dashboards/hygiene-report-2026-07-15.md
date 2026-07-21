# Hygiene Report — 2026-07-15

Run at 11:28 GMT+3 (delayed run — see changelog; janitor was due 9:30am).
Book size: 46 local account folders.

---

### Job 1: Context Freshness

✅ Fresh (≤7d): 3 accounts — Fireblocks, Lemonade, MyHeritage Ltd. main account
⚠️ Stale (7-14d): 14 accounts — GameStory Israel (13d), AT&T Israel (9d), Nanit (9d), PeerPlay (9d), Tabnine (Previously Codota) (9d), shortical.com (9d), Fibi - Israeli International Bank (8d), Forex Club International LLC (8d), Kape Technologies PLC (Israel) (8d), Nexus Mods (8d), PayBox - Discount Bank (8d), Solaredge (8d), Taboola (8d), boinkers.io BY Acid Labs (8d)
🔴 Critical (>14d): 30 accounts — Appcharge LTD, Base44, BlueThrone, ClickSoftware, Duda, Movement-Group, Papaya Global {Formerly Azimo}, Talon Cyber Security, mbg-digital.com (all 42d); Bookaway, Ferryhopper, Loora.AI, Pango Pay & Go ltd, RubyLabs, Simply, Stillfront Group AB (all 35d); GETT (34d); Altshuler Shaham, AutoDS IL, HAAT Delivery, Riverside.fm, SimilarWeb, StakeMate, Whalo, runwayer.com (all 24d); DuxGroup, Gamdom, Guard.io (23d); Elementor, HoneyBook (15d)

**Only 3/46 accounts (6.5%) have been re-written by CSA OS in the last 7 days.** This is the single biggest structural problem this run — most of the book's local `_context.md` files predate the last several morning cycles' Notion writes, meaning file-based views (including this janitor run's Job 6/7 analysis below) are working off stale snapshots for 43 of 46 accounts.

---

### Job 2: Source Validation

Full validation of all 46 accounts' Sources tables (Slack, Notion, SFDC, GDoc, BQ) was out of scope for this run given time constraints — see Notes. Spot-checked instead:
- **Notion:** Accounts DB (`cfe01032...`) — accessible. Context pages for AT&T Israel and Lemonade — both fetched successfully, render correctly.
- **Slack:** #at-clicksoftware — channel accessible, but its most recent message (2026-07-12, an automated Weekly Account Summary) is newer than the manifest's `Last Fetched` (2026-07-02) — the channel itself is fine, but confirms the harvester has fallen behind here (consistent with the Job 1 freshness gap).

✅ All spot-checked sources accessible: 3/3
⚠️ Access issues: none found in the spot-check, but coverage was partial — treat as inconclusive, not clean, for the 43 accounts not checked.

---

### Job 3: Inbox

Pending: 2 items (both older than the `.processed` folder, which is empty)
- `2026-05-27-dm-notes.md` — "Check even import errors walo" — likely **Whalo** (probable truncated/typo match on account name), previously logged as unmatched. 49 days old and still unprocessed.
- `2026-06-30-dm-notes.md` — "gong is set up" — infrastructure note (Clari→Gong migration confirmation), not account-specific. 15 days old and still unprocessed.

Neither has been filed to `.processed/` despite being referenced/resolved in other logs — inbox processing appears to not be running as part of the cycle, or is running but not marking items processed.

---

### Job 4: Structural Hygiene + Drift

✅ 46/46 accounts have both `_context.md` and `_MANIFEST.md`
⚠️ Orphaned folder: **Fireblocks** — exists locally with no `ACCOUNT:Fireblocks` entry in `resources/notion-page-registry.md`. Fireblocks' own `_context.md` also self-describes as a "stub account" with no Slack channels, no call/email history, and no BQ pull yet, plus a Renewal Risk of "Churned — confirmed 2026-06-21." This looks like a dead/should-be-archived account that was never fully onboarded or registered — recommend confirming with Yonah whether to archive it or complete its registry entry.
✅ No unfilled `{PLACEHOLDER}` values in `overrides.md`
✅ `cadence.md` parses cleanly

**Drift check:** Full reconcile across 46 accounts was out of scope this run (see Notes). Spot-checked AT&T Israel and Lemonade Notion Context pages against local `_context.md` — content is broadly consistent (Notion has more granular history than the local snapshot, as expected given Job 1's freshness gap, but no contradictory/conflicting facts found). Two known drift issues are already self-documented in-file and not new findings: DuxGroup's Notion "Renewal Date" property (shows 2026-06-30) vs. its Context page (FY27 renewal actually Closed Won 2026-06-20, $78,810) — a property-sync gap, not a real lapsed renewal; and a similar child-page-to-parent-DB rollup gap noted on PayBox.
✅ 2/2 spot-checked accounts in sync (partial coverage — not a full-book claim)

---

### Job 5: Notion Health

✅ Accounts DB accessible (`cfe01032176e455eb849315deff4a68c`, 3 views: Accounts/Tal/Shavit)
✅ 2/2 spot-checked pages OK (AT&T Israel Context, Lemonade Context)
⚠️ Stale registry entries: none found in spot-check, not exhaustively checked

---

### Job 6: Upcoming Renewals + Risk Flags

🔴 **Renewal lapsed / imminent:**
- **GETT** — Contract End 2026-05-31, now **45 days lapsed**, renewal still Stage 1: Pipeline ($65,500 FY28 opp)
- **AT&T Israel** — FY27 Renewal ($63,205) expired 2026-06-30, now **15 days lapsed**, still unresolved per local notes (legal hold / SecurityScorecard review blocking; last local update said escalation miscast, needs correct decision-maker identified)
- **Guard.io** — renewal moved to Stage 4: Negotiation, $81,469, close date noted as "5 days out" as of its last update (2026-06-22) — given file staleness (23d), this close date should be treated as unverified and re-confirmed this cycle

⚠️ **Critical health / high risk (Health: Low + Risk: High, or Renewal Risk: High):**
AT&T Israel, ClickSoftware, PayBox - Discount Bank, PeerPlay, Tabnine (Previously Codota), shortical.com (Health Low/Risk High); Altshuler Shaham, Ferryhopper, Fibi - Israeli International Bank, Gamdom, Movement-Group, Taboola, Talon Cyber Security (Renewal Risk: High)

⚠️ **Overdue open items (>7 days):** **100 unchecked open items across 39 accounts.** Worst offenders: Ferryhopper, Nexus Mods, Base44, Taboola (44d overdue since 2026-06-01); Appcharge LTD, Tabnine (43d); Simply, Lemonade, SimilarWeb (42d). Full list in changelog detail.

Note: Fireblocks is separately flagged as **already churned** (confirmed 2026-06-21), not a <60d renewal risk — see Job 4 orphaned-folder note.

---

### Job 7: Communication Silence

Computed using each account's actual `Last Engaged` (Contacts table, `_MANIFEST.md`) and `Recent History` dates (`_context.md`) — not file-freshness/harvest-bookkeeping dates, per the Job 7 distinction.

🔴 **High Priority Outreach (46+ days silent): 1 account**
- **ClickSoftware** — 171d since last tracked customer contact (2026-01-25) — try: the account's own weekly Slack summary (posted 2026-07-12) already flags 13+ consecutive weeks of zero named-user activity and a single unresponsive contact (Vince Valentino); use that summary itself as the trigger to escalate to a second contact (IT admin/procurement) rather than re-trying Vince a fourth time.

⚠️ **Needs Outreach (22-45 days silent): 7 accounts**
- **Movement-Group** — 44d (2026-06-01) — try: the unresolved myIQ implementation issue and data-flow-diagram request for the security team are still open (both due 2026-06-05/10) — lead with unblocking myIQ, it's the concrete reason they've gone quiet.
- **DuxGroup** — 35d (2026-06-10) — try: Session Replay upsell has stalled since the Apr 14 demo — re-open with a direct check-in on SR, not a generic touch base.
- **Ferryhopper** — 35d (2026-06-10) — try: nested-properties/constant-filters limitation is still blocking Daniel's use case and usage is siloed to one champion (Thodoris) — worth widening the relationship while re-engaging on the technical blocker.
- **Loora.AI** — 35d (2026-06-10) — try: GDPR EU data-residency guidance was promised to Yonti and never sent — close that loop as the re-engagement opener.
- **Gamdom** — 30d (2026-06-15) — try: the Aug 1 data-warehouse-connector discussion was deferred to "early August" — worth a pre-check with Connor now rather than waiting, especially given prior severe underutilization risk on this account.
- **runwayer.com** — 24d (2026-06-21) — try: "Project Stopped Sending Data" alert has been open/unassigned since May 29 — this is a live product-health issue, not a relationship check-in; lead with it.
- **Guard.io** — 23d (2026-06-22) — try: renewal reportedly moved to Stage 4: Negotiation with a close date "5 days out" as of the last update — confirm status now given the account has gone quiet since.

**Exempt — Verify Manually (untracked primary channel): 1 account**
- **AT&T Israel** (WhatsApp) — 9d since last TRACKED contact — confirm the WhatsApp relationship is still active; also cross-reference with the lapsed-renewal flag in Job 6, since WhatsApp being the primary channel may mean real customer contact around the stalled renewal isn't visible to CSA OS at all.

---

## Notes on Scope

This run scoped down full-book live validation (Job 2 source checks, Job 4 full reconcile, Job 5 exhaustive registry check) to a small spot-check (2-3 accounts/sources) given the size of the book (46 accounts) and the time budget for this run. Jobs 1, 3, 4 (structural), 6, and 7 were run against the full local book (all 46 accounts) via direct file analysis, which is authoritative for those checks. No MCP was unavailable — Notion and Slack both responded successfully wherever queried; the narrower scope was a deliberate time/cost tradeoff, not a connectivity failure.
