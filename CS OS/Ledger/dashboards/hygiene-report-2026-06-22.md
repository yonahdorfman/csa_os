# Hygiene Report — 2026-06-22

**Generated:** 10:12 IDT
**Accounts checked:** 45
**Warnings:** 8

---

### Job 1: Context Freshness

✅ Fresh (≤7d): 3 accounts
- Gamdom (Jun 16), AT&T Israel (Jun 15), Nexus Mods (Jun 15)

⚠️ Stale (7-14d): 14 accounts
- GETT (Jun 11), Bookaway (Jun 10), Elementor (Jun 10), Ferryhopper (Jun 10), Loora.AI (Jun 10), MyHeritage (Jun 10), Pango (Jun 10), RubyLabs (Jun 10), Simply (Jun 10), Shortical (Jun 10), Stillfront (Jun 10), Taboola (Jun 10), Whalo (Jun 10), boinkers.io (Jun 10)

🔴 Critical (>14d): 28 accounts
- All accounts last updated 2026-06-03 (19 days): Altshuler Shaham, Appcharge, AutoDS IL, Base44, BlueThrone, ClickSoftware, Duda, DuxGroup, Fibi, Forex Club, GameStory Israel, Guard.io, HAAT Delivery, HoneyBook, Kape Technologies, Lemonade, mbg-digital.com, Movement-Group, Nanit, Papaya Global, PayBox, PeerPlay, Riverside.fm, runwayer.com, SimilarWeb, Solaredge, StakeMate, Tabnine, Talon Cyber Security

Note: DuxGroup has a CLOSED WON signal (Jun 20, $78,810) in Slack — local _context.md predates the close. Morning cycle write will update.

---

### Job 2: Source Validation

✅ Slack channels readable — spot-checked 10 channels this cycle (AT&T, DuxGroup, Elementor, Lemonade, Kape, MyHeritage, SimilarWeb, HoneyBook, Taboola, Papaya Global): all returned data
✅ Notion registry present (45 entries, DB cfe01032176e455eb849315deff4a68c)
⚠️ BQ query result exceeded token limit this cycle — account roster from BQ unavailable. Workaround: Slack weekly summaries provided ARR/health/stage data for key accounts.
⚠️ Gmail harvest returned empty (no unread messages in 24h window) — may indicate Gmail MCP auth lapse or genuine inbox silence

---

### Job 3: Inbox

Pending: 1 item
- `2026-05-27-dm-notes.md` — unprocessed (26 days old, triage overdue)

---

### Job 4: Structure + Sync

✅ 45/45 accounts have complete file sets (_context.md + _MANIFEST.md)
✅ cadence.md present and parseable
✅ overrides.md has no unfilled placeholders

⚠️ 28 accounts critical (>14d) — up from 0 critical on Jun 17. Morning cycle Notion pull + write will address.

Drift check: Local files are working snapshots; Notion is source of truth. Significant drift expected for Jun 3 cohort. DuxGroup local context stale by ~19 days; SFDC CLOSED WON Jun 20 not yet reflected locally.

---

### Job 5: Notion Health

✅ Accounts DB accessible (cfe01032176e455eb849315deff4a68c)
✅ Registry entries present for all 45 accounts
Spot-check: Based on prior cycle reads — registry intact

---

### Job 6: Alerts

🔴 EXPIRED RENEWAL: Lemonade — May 31 (22 days past due, ~$80K, no active deal)
🔴 Renewal <14d: AT&T Israel — Jun 30 (8 days, $63K, Stage 4: Negotiation, Ayelet silent post-Jun 15 session)
✅ DuxGroup — CLOSED WON Jun 20 ($78,810, +27.5% uplift) — removed from alert list
⚠️ Renewal <60d: Elementor — Jul 31 (39 days, $117K, Stage 4: Negotiation, Ghaith watching, Hagay checklist overdue)
⚠️ Renewal <60d: MyHeritage — Jul 31 (39 days, ~$110K, renewal not started)
⚠️ Renewal <60d: GameStory Israel — Aug 1 (40 days)
⚠️ High risk: Talon Cyber Security (near-zero engagement, Oct renewal)
⚠️ High risk: ClickSoftware (zero product adoption, Jan 2027 renewal)
⚠️ High risk: PeerPlay (P1 incident unresolved, Sep renewal)
⚠️ High risk: Tabnine (MTU contract dispute, billing team owns)
⚠️ Inbox item 26 days old — triage needed
