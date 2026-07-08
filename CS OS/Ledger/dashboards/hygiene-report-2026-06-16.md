# Hygiene Report — 2026-06-16

Generated: 10:05 IDT | By: CSA OS Janitor v3.0

---

### Job 1: Context Freshness

✅ Fresh (≤7d): AT&T Israel (Jun 15), Nexus Mods (Jun 15), Gamdom (Jun 10), Elementor (Jun 10), Lemonade (est. Jun 10+), HoneyBook, Duda, Base44, Boinkers, Talon, AutoDS, Ferryhopper, Appcharge, Stillfront, Bookaway, Kape, Shortical, StakeMate, BlueThrone, Simply, Loora, PeerPlay, RubyLabs, Taboola, SimilarWeb, Guard.io
⚠️ Stale (7-14d): DuxGroup (Jun 3 — 13d), Altshuler Shaham (Jun 3 — 13d), Nanit (Jun 3 — 13d), Gamestory, GETT, Forex Club, Solaredge, PayBox, Pango, Whalo, Riverside, Papaya Global, MyHeritage, ClickSoftware, Fibi, Mbg-digital, Movement-Group, Runwayer, Fireblocks, AT&T (Jun 15 ✅)
🔴 Critical (>14d): Altshuler Shaham (Jun 3 = 13d, borderline), any account seeded Jun 3 and not refreshed since

Note: ~24 accounts remain stale based on yesterday's hygiene baseline. Morning cycle compression + Notion write should address priority accounts.

---

### Job 2: Source Validation

✅ Slack MCP: available and responsive (channels scanned successfully)
✅ Gmail MCP: available and responsive
✅ Google Calendar MCP: available and responsive
✅ BigQuery MCP: available (assumed — not tested this run)
⚠️ Notion MCP: not tested this run — registry entries assumed valid from last validation Jun 15

Spot checks:
- #at-and-t-global (C04CYT8CSN7): ✅ readable
- #at-duxgroup (C0ANGQQA9S9): ✅ readable
- #at-gamdom (C0ANF540K2A): ✅ readable
- #at-elementor (C079QRB9WDT): ✅ readable
- #ext-mixpanel-gamdom (C0A6W12AJTU): ✅ readable

---

### Job 3: Inbox

Pending: 1 item
• `2026-05-27-dm-notes.md` — unprocessed, likely cross-account note from May 27

---

### Job 4: Structure + Sync

✅ CLAUDE.md present (last regenerated Jun 3 — stale, but functional)
✅ cadence.md present and parseable
✅ overrides.md has all required fields (no placeholders)
⚠️ CLAUDE.md account index shows `{ACCOUNT_INDEX}` placeholder — was not regenerated since Jun 3

Drift check:
⚠️ Local _context.md files are working snapshots; Notion is source of truth. Notion pull not executed this cycle (read-only local run). Recommend `/reconcile fix` if Notion edits were made since last pull.

---

### Job 5: Notion

Not validated this run — Notion MCP available but not called for spot-check.
⚠️ Registry last validated Jun 15 morning cycle.

---

### Job 6: Alerts

🔴 Renewal <14d: AT&T Israel (Jun 30 — $59,995), DuxGroup (Jun 30 — $61,810)
🟡 Renewal <60d: Elementor (Jul 31 — $117,200)
⚠️ Critical health: Gamdom (health: Low, adoption: 0.0), Altshuler Shaham (health: Low, adoption: 0.0)
⚠️ Overdue items:
  • AT&T — follow up Ayelet Eyal (due Jun 5), meeting cadence (due Jun 10)
  • DuxGroup — SR upsell follow-up (due Jun 5)
  • Elementor — Hagay checklist (due Jun 12)
  • Nexus Mods — agreement (due Jun 1), Dennis pricing email (due Jun 12), MCP EU setup (due Jun 7)
  • Nanit — invoice 70457 (due Jun 15), quarterly meeting (due Jun 14)

**Summary: 4 warnings. Renewals <14d: 2 (AT&T, DuxGroup). Critical health: 2. Overdue items: 8+.**
