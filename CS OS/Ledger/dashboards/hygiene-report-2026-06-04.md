# CSA OS — Hygiene Report
**Date:** 2026-06-04
**Run by:** janitor v3.0 (scheduled, 7:30am)
**Accounts scanned:** 46
**Total warnings:** 15

---

### Job 1: Context Freshness

✅ Fresh (≤7d): **46 accounts** — all updated 2026-06-03 (yesterday, BQ seed)
⚠️ Stale (7-14d): none
🔴 Critical (>14d): none

---

### Job 2: Source Validation

✅ All sources accessible: **5/5 spot-checked**

Spot-checked channels:
- CHE9ZMQM8 (#at-lemonade) ✓
- C06CFJ53X8A (#mixpanel-lemonade-shared) ✓
- CLABN6TLK (#at-similarweb) ✓
- C065M6ZAD6F (#at-tabnine) ✓
- C0ANJ1YUVHC (#at-honeybook) ✓

Notion DB (cfe01032176e455eb849315deff4a68c) ✓ accessible
BlueThrone context page (374e0ba9-2562-8159-b5e4-e734b9c4db66) ✓ accessible

⚠️ Access issues: none detected (skipped: SFDC, Drive — not connected)

---

### Job 3: Inbox

⚠️ Pending: **1 item**
- `2026-05-27-dm-notes.md` — unmatched DM ("Check even import errors walo"), 8 days old, no account match found. Possible truncated account name (Whalo?). Requires manual review.

---

### Job 4: Structure + Sync

✅ 46/46 accounts have complete file sets (_context.md + _MANIFEST.md)
✅ cadence.md exists and parseable
✅ overrides.md — no unfilled {PLACEHOLDER} values

⚠️ Duplicate folder detected:
- `Accounts/Papaya Global -Formerly Azimo-/` (legacy)
- `Accounts/Papaya Global {Formerly Azimo}/` (active, matches registry pattern)
- Registry entry: `ACCOUNT:Papaya Global (Formerly Azimo)` — name mismatch on both folders. Run cleanup to remove legacy folder and align naming.

⚠️ Missing renewal dates (Identity table shows "—"):
- Bookaway ($46,647 ARR)
- Stillfront Group AB ($12,348 ARR)
- Papaya Global (Formerly Azimo) ($42,800 ARR)

Drift check:
✅ 1/1 spot-checked account (BlueThrone) — local and Notion in sync
✅ All 46 contexts show Last Updated: 2026-06-03 (consistent with yesterday's morning cycle)
No drift detected.

---

### Job 5: Notion

✅ Accounts DB accessible (cfe01032176e455eb849315deff4a68c)
✅ 2/2 spot-checked pages OK (DB schema + BlueThrone:context)
⚠️ Stale registry entries: none detected

---

### Job 6: Alerts

🔴 **Renewal <60d:**
- AT&T Israel — 2026-06-30 (26 days) | $63,205 ARR | Stage 4: Negotiation
- DuxGroup — 2026-06-30 (26 days) | $79,000 ARR | Active negotiation, SR upsell stalled
- Elementor — 2026-07-31 (57 days) | $117,200 ARR | MTU mismatch risk; Intro meeting today Jun 4
- MyHeritage Ltd. — 2026-07-31 (57 days) | $110,000 ARR | Active pipeline
- GameStory Israel — 2026-08-01 (58 days) | $50,400 ARR | Strong engagement

⚠️ **Critical health / churn risk:**
- ClickSoftware — severe risk; near-zero usage (51 queries, no named users 5+ weeks); immediate activation required
- Tabnine (Previously Codota) — explicitly threatening churn; customer demands contract restructure (B2C→B2B pricing); do not let this drift
- Talon Cyber Security — severe risk; 995 queries across 7 users; Palo Alto acquisition may lead to tool consolidation; tentative lunch with Adi is the make-or-break next step
- Gamdom — critically low usage (223 queries, 6 users); only 0.4yr customer; activation outreach and monthly office hours needed

⚠️ **High risk health:**
- Ferryhopper — low health; single-user dependency; nested property blockers unresolved
- runwayer.com — data stopped sending; alert open; immediate technical verification needed
- mbg-digital.com — low health; billing/payment dispute blocking access; onboarding blocked

⚠️ **Overdue items:**
- Nexus Mods — "Nexus Mods Agreement" due 2026-06-01, now 3 days overdue (Owner: Yonah)

---

## Summary

| Category | Count |
|---|---|
| Fresh accounts | 46/46 |
| Stale accounts | 0 |
| Critical (context) | 0 |
| Broken sources | 0 |
| Drift (local↔Notion) | 0 |
| Structural issues | 2 (duplicate folder, 3 missing renewal dates) |
| Inbox pending | 1 |
| Renewals <60d | 5 |
| Critical churn risk | 4 |
| High risk | 3 |
| Overdue items | 1 |

**Worst issue:** 4 accounts at critical churn risk (ClickSoftware, Tabnine, Talon, Gamdom) + 2 renewals in 26 days (AT&T Israel, DuxGroup).
