# Hygiene Report — 2026-06-25

Generated: 09:54 IDT by CSA OS Janitor v3.0

---

### Job 1: Context Freshness

✅ Fresh (≤7d): 21 accounts
AT&T Israel, Altshuler Shaham, AutoDS IL, DuxGroup, Fibi, Fireblocks, Gamdom, Guard.io, HAAT Delivery, HoneyBook, Kape Technologies, Nanit, PayBox - Discount Bank, PeerPlay, Riverside.fm, SimilarWeb, Solaredge, StakeMate, Whalo, runwayer.com, shortical.com

⚠️ Stale (7-14d): 2 accounts
- Nexus Mods (Jun 15 — 10 days)
- GETT (Jun 11 — 14 days)

🔴 Critical (>14d): 24 accounts
- Bookaway, Elementor, Ferryhopper, Loora.AI, MyHeritage, Pango Pay & Go, RubyLabs, Simply, Stillfront Group AB, Taboola, boinkers.io BY Acid Labs (Jun 10 — 15 days)
- Appcharge LTD, Base44, BlueThrone, ClickSoftware, Duda, Forex Club, GameStory Israel, Lemonade, Movement-Group, Papaya Global, Tabnine, Talon Cyber Security, mbg-digital.com (Jun 3 — 22 days)

---

### Job 2: Source Validation

✅ All accounts have _context.md and _MANIFEST.md
✅ Notion registry has entries for all local accounts (47 accounts, 47 registry entries)
⚠️ Slack source validation: ClickSoftware channel active (weekly summaries posting); MyHeritage external — no customer reply since May 10 (46 days)
⚠️ BQ validation: skipped this cycle (MCP available but not queried for freshness check)

---

### Job 3: Inbox

Pending: 1 item
- 2026-05-27-dm-notes.md — "Check even import errors walo" — unmatched (likely Whalo, but event import errors unresolved)

---

### Job 4: Structure + Sync

✅ 47/47 accounts have complete file sets
✅ All accounts have Notion registry entries
⚠️ Drift check: 24 accounts have local contexts >14d old — Notion may be more current for accounts with BQ-seeded updates. Recommend /reconcile check.
⚠️ MyHeritage local context (Jun 10) is stale vs Notion which may have Jun 23 EoD retro entry

---

### Job 5: Notion Health

✅ Notion MCP available
✅ Registry accessible (47 account entries verified)
⚠️ Not spot-checked pages this cycle (time constraint — focus on harvest and briefing)

---

### Job 6: Alerts

🔴 Renewal <5d: AT&T Israel (Jun 30, $63,205) — legal hold active, passive posture
🔴 Renewal <37d: MyHeritage (Jul 31, $110,000)
🔴 Renewal <37d: Elementor (Jul 31, $117,200)
⚠️ Critical health: ClickSoftware — zero usage 10+ weeks post-renewal
⚠️ Critical health: Altshuler Shaham — 181 queries/7d, single-user dependency (Sari)
⚠️ Overdue items:
  • AT&T Israel — follow-up with Ayelet Eyal (due Jun 5, now 20 days overdue)
  • AT&T Israel — schedule meeting cadence (due Jun 10, overdue)
  • PeerPlay — incident summary for Guy Zamir (overdue since Jun 17, 8 days)
  • Kape — check-in with champion (due Jun 7, overdue)
  • Kape — SR + Custom Alerts intro (due Jun 14, overdue)
  • BlueThrone — confirm MTU count for FY28 (due Jun 15, overdue)
  • MyHeritage — confirm Itay Reznik funnel discrepancy resolution (due Jun 11, overdue)
⚠️ Plan usage alerts:
  • shortical.com: 117.88% — OVERAGE
  • Guard.io: 97.6% — approaching limit (Stage 4: Negotiation)
  • DuxGroup: 97.9% — approaching limit (just renewed Jun 20)
  • Nanit: 90.7% — approaching limit
  • PeerPlay: 90.8% — approaching limit

**Janitor result: WARNING — 7 overdue items, 5 plan usage alerts, 2 renewals ≤37d, 1 renewal ≤5d, 24 critical stale accounts**
