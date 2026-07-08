# Hygiene Report — 2026-06-09

**Generated:** 2026-06-09 07:30 IDT (scheduled)
**Book:** 46 accounts | **Warnings:** 16

---

### Job 1: Context Freshness

✅ Fresh (≤7d): 46 accounts — all updated 2026-06-03 (6 days ago)
⚠️ Stale (7-14d): none
🔴 Critical (>14d): none

---

### Job 2: Source Validation

Spot checks (Slack + Notion):
- Lemonade #CHE9ZMQM8 ✅
- DuxGroup #C0ANGQQA9S9 ✅
- SimilarWeb #CLABN6TLK ✅
- Notion: Lemonade Context page (374e0ba9…fc1a) ✅
- Notion: Guard.io Context page (374e0ba9…0618) ✅
- Notion Accounts DB (cfe01032…) ✅

✅ All sources accessible: 46 accounts (spot-checked 5 sources)
⚠️ Access issues: none detected

---

### Job 3: Inbox

Pending: 1 item
• `2026-05-27-dm-notes.md` — likely general DM notes (no `.processed/` folder exists)

---

### Job 4: Structure + Sync

✅ 46/46 accounts have complete file sets (_context.md + _MANIFEST.md)
✅ 46/46 accounts have Notion registry entries
⚠️ Naming mismatch: Papaya Global — folder uses `{Formerly Azimo}` but registry uses `(Formerly Azimo)` (cosmetic, low priority)
⚠️ Roster gap: Fireblocks (billing ID 4022185) appears in CSM account list but has no local folder or registry entry — may be unmanaged or offboarded

Drift check:
✅ No local/Notion drift detected in spot checks (both seeded 2026-06-03)
ℹ️ Full reconcile not run — spot checks clean; run `/reconcile check` if drift is suspected

---

### Job 5: Notion

✅ Accounts DB accessible (cfe01032176e455eb849315deff4a68c)
✅ 2/2 spot-checked pages OK (Lemonade, Guard.io)
✅ No stale registry entries detected in spot checks

---

### Job 6: Alerts

**Renewals <60 days:**
🔴 AT&T Israel — 2026-06-30 (21 days) | Risk: Medium | Negotiation active; user ID friction unresolved
🔴 DuxGroup — 2026-06-30 (21 days) | Risk: Medium | **No meeting scheduled** — critical gap
⚠️ Elementor — 2026-07-31 (52 days) | Risk: Medium | MTU gap (800K vs 1.5M plan) is commercial risk
⚠️ MyHeritage — 2026-07-31 (52 days) | Risk: Low | Active pipeline; Metric Trees upsell in S2
⚠️ GameStory Israel — 2026-08-01 (53 days) | Risk: Low | Billing dispute needs resolution first
📌 Lemonade — Renewal date 2026-05-31 is past — context shows Closed Won at $80,284; stale field in _context.md

**Critical health / churn risk:**
🔴 Tabnine — explicitly threatening churn; demands repricing from 3M MTU legacy contract; won't renew as-is
🔴 ClickSoftware — near-zero usage (51 unattributed queries), zero named users 5+ weeks, activation overdue
🔴 Talon Cyber Security — Palo Alto acquisition → near-zero engagement; active churn risk
⚠️ Gamdom — critically low usage (223 queries / 6 users); new customer; office hours not yet scheduled
⚠️ Ferryhopper — single-user dependency; nested property blockers; low health
⚠️ mbg-digital.com — billing dispute blocking access; low health; early onboarding stalled

**Overdue action items (>7 days):**
🔴 Ferryhopper — [DUE 2026-06-01] Talk with Max re account strategy (Owner: Yonah) — **8 days overdue**
🔴 Nexus Mods — [DUE 2026-06-01] Resolve Nexus Mods Agreement (Owner: Yonah) — **8 days overdue**
🔴 runwayer.com — [DUE 2026-06-02] Investigate Project Stopped Sending Data alert (Owner: Yonah) — **7 days overdue**
🔴 Appcharge LTD — [DUE 2026-06-02] Resolve Pylon Issue #927 SLA breach (Owner: Yonah) — **7 days overdue**
🔴 StakeMate — [DUE 2026-06-02] Send power analysis tool to Oliver (Owner: Yonah) — **7 days overdue**

**Additional overdue items (2-6 days):**
⚠️ ClickSoftware — [DUE 2026-06-07] Identify product champion (Owner: Shavit) — 2 days
⚠️ AutoDS IL — [DUE 2026-06-07] Resolve secondary invoice (Owner: Tal) — 2 days
⚠️ Kape Technologies — [DUE 2026-06-07] Schedule champion check-in (Owner: Yonah) — 2 days
⚠️ Loora.AI — [DUE 2026-06-07] Send GDPR/data residency guidance to Yonti (Owner: Yonah) — 2 days
⚠️ Movement-Group — [DUE 2026-06-05] Resolve myIQ implementation issue — 4 days
⚠️ BlueThrone — [DUE 2026-06-05] Secure WHC trial approval (Owner: Shavit) — 4 days
⚠️ Forex Club — [DUE 2026-06-05] Schedule proactive check-in with Oleksiy (Owner: Yonah) — 4 days
⚠️ Ferryhopper — [DUE 2026-06-05] Consult Daniel re nested properties (Owner: Yonah) — 4 days
⚠️ RubyLabs — [DUE 2026-06-05] Send Mixpanel Headless links + SCIM follow-up (Owner: Shavit/Yonah) — 4 days
⚠️ StakeMate — [DUE 2026-06-05] Open SR trial (Owner: Tal) — 4 days
⚠️ Appcharge LTD — [DUE 2026-06-05] Chase WHI trial approval (Owner: Tal) — 4 days
⚠️ Talon Cyber Security — [DUE 2026-06-05] Confirm Adi lunch (Owner: Tal) — 4 days

---

## Summary

| Metric | Count |
|---|---|
| Total warnings | 16 |
| Fresh accounts | 46/46 |
| Stale/Critical | 0 |
| Broken sources | 0 |
| Renewals <60d | 5 (+1 past due) |
| Critical health | 6 |
| Overdue >7d | 5 |
| Overdue 2-6d | 11 |
| Inbox pending | 1 |
| Drift | 0 detected |
