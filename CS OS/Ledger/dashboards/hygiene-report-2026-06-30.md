# Hygiene Report — 2026-06-30
Generated: 10:08 IDT

---

### Job 1: Context Freshness

✅ Fresh (≤7d): 1 account
- PeerPlay (Jun 23)

⚠️ Stale (7-14d): 20 accounts
AT&T Israel, DuxGroup, Gamdom, Guard.io, Nanit, shortical.com (all Jun 22); Altshuler Shaham, AutoDS IL, Fibi, Fireblocks, HAAT Delivery, HoneyBook, Kape Technologies, PayBox, Riverside.fm, SimilarWeb, Solaredge, StakeMate, Whalo, runwayer.com (all Jun 21)

🔴 Critical (>14d): 26 accounts
Appcharge, Base44, BlueThrone, ClickSoftware, Duda, Forex Club, GameStory Israel, Lemonade, Movement-Group, Papaya Global, Tabnine, Talon, mbg-digital.com (all Jun 3 — 27d); Bookaway, boinkers.io, Elementor, Ferryhopper, Loora.AI, Pango, RubyLabs, Simply, Stillfront, Taboola (all Jun 10 — 20d); GETT (Jun 11 — 19d); MyHeritage (Jun 10 — 20d); Nexus Mods (Jun 15 — 15d)

Note: High critical count reflects harvest-only updates for accounts with no signals since last compress cycle. Morning cycle BQ refresh will update Sales Intelligence sections today.

---

### Job 2: Source Validation

Slack channels: Spot-checked #at-and-t-global (C04CYT8CSN7) ✅, #at-honeybook (C0ANJ1YUVHC) ✅, #at-peerplay (C0ANPNNT2HJ) ✅, #at-elementor (C079QRB9WDT) ✅ — all accessible.

BigQuery: ✅ account_metadata and account_plans_current both responsive (full query completed).

Gmail: ✅ accessible, 0 unread messages in last 24h.

Google Calendar: ✅ responsive.

Clari/Gong: ⚠️ Clari retires TOMORROW (Jul 1). Gong setup status unknown — verify before EOD.

Fireblocks: ⚠️ No Slack channel ID mapped in BQ or registry. Source validation skipped.

---

### Job 3: Inbox

Pending: 1 item
- `2026-05-27-dm-notes.md` — unmatched/historical DM notes from May 27

DM Monitor cursor stale: last processed 2026-06-21T11:15. One unprocessed DM note found:
- Jun 29 15:38 — Yonah: "For honeybook I need to send AI and skill docs" → Filed to HoneyBook open items.

---

### Job 4: Structure + Sync

✅ 47/47 account folders have _context.md and _MANIFEST.md (spot-check passed)
✅ Notion registry has entries for all accounts
⚠️ Registry uses "Wilma Partners Limited" but local folder is "DuxGroup" — consistent in practice, minor naming drift
⚠️ DM monitor cursor has not been updated since Jun 21 — 9 days of potential missed DM notes

Drift check: Full reconcile not run (no reconcile MCP available). Local files are pull-from-Notion snapshots — considered in sync with last cycle.

---

### Job 5: Notion Health

✅ Registry has 47 accounts with complete page ID sets
✅ BQ data source accessible and returning fresh data (last refresh: Jun 28-29 per executive summaries)
⚠️ Bookaway and Papaya Global missing from account_plans_current (BQ returns null for plans data) — context is BQ metadata only

---

### Job 6: Alerts

🔴 Renewal TODAY: AT&T Israel (Jun 30, $63K, Stage 4: Negotiation) — legal hold as of Jun 22
🔴 Renewal <31d: MyHeritage (Jul 31, $110K), Elementor (Jul 31, $107K Stage 5: Signing), PeerPlay (Jul 31, $91K Stage 4: Negotiation)
⚠️ Renewal <60d: GameStory Israel (Aug 1, $50K), Whalo (Aug 31, $130K)
🔴 Churn risk: Tabnine — champion threatening churn, auto-renewal stopped; shortical.com — 117% overage, customer considering disabling Mixpanel
🔴 Zero usage: ClickSoftware (10+ weeks), Talon (acquired by Palo Alto, near-zero)
⚠️ Quota alerts: Guard.io (97.2% event quota), DuxGroup (97.9% post-renewal)
⚠️ Downgrade risk: PayBox ($256K downgrade in Stage 4 Buying Process)
⚠️ Infrastructure: Clari retires TOMORROW (Jul 1) — Gong must be fully configured today
⚠️ Overdue items (critical): PeerPlay incident summary (13d), Elementor Hagay checklist (17d), Pango funnel question (26d), SimilarWeb Sapir outreach (14d), Fibi EU endpoint (2d)

**Total warnings: 11 critical, 8 advisory**
