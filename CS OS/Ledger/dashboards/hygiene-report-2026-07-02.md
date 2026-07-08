# Hygiene Report — 2026-07-02

### Job 1: Context Freshness

✅ Fresh (≤7d): 2 accounts — Elementor (Jun 30), HoneyBook (Jun 30)
⚠️ Stale (7-14d): 20 accounts — AT&T Israel, PeerPlay, shortical.com, DuxGroup, Gamdom, Guard.io, Nanit, Altshuler Shaham, AutoDS IL, Fibi, Fireblocks, HAAT Delivery, Kape Technologies, PayBox, Riverside.fm, SimilarWeb, Solaredge, StakeMate, Whalo, runwayer.com
🔴 Critical (>14d): 25 accounts — Nexus Mods, Bookaway, Ferryhopper, GETT, Loora.AI, MyHeritage, Pango Pay & Go, RubyLabs, Simply, Stillfront Group, Taboola, boinkers.io, Appcharge, Base44, BlueThrone, ClickSoftware, Duda, Forex Club, GameStory Israel, Lemonade, Movement-Group, Papaya Global, Tabnine, Talon Cyber Security, mbg-digital.com

Only 2/47 accounts have been touched by a harvest in the last week. This tracks with repeated Slack/Notion MCP auth failures logged on the 07:11 and 09:01 ticks today — the daemon has been unable to harvest or write context for most of the book while those connectors were down.

### Job 2: Source Validation

Not run at full per-account depth this cycle (scope-limited). Spot check: Notion search and Slack DM read both succeeded this tick (both were 403/unauthenticated as of 09:01) — core MCPs are back online as of ~09:45 IDT. Recommend a full source validation pass next janitor run now that access is restored.

### Job 3: Inbox

Pending: 2 items
• 2026-06-30-dm-notes.md — "gong is set up" — infra note, no account, already actioned (Clari→Gong migration confirmed complete)
• 2026-05-27-dm-notes.md — "Check even import errors walo" — likely truncated reference to **Whalo** (36 days unprocessed). Needs manual confirmation and filing — flagged for 3 straight janitor runs now.

### Job 4: Structure + Sync

✅ 47/47 accounts have complete file sets (_context.md + _MANIFEST.md present)
Drift check not run this cycle (requires per-account Notion fetch — deferred to next janitor pass given the connector outage earlier today).

### Job 5: Notion

✅ Accounts DB / workspace search accessible (confirmed working this tick after this morning's outage)

### Job 6: Alerts

🔴 Renewal passed, unresolved: **AT&T Israel** — expired 2026-06-30 ($63,205), Stage 4 Negotiation, legal hold, no confirmed outcome as of this morning
🔴 Renewal <30d: **Elementor** ($107K, 2026-07-31, Stage 5 Signing) · **MyHeritage** ($110K, 2026-07-31)
⚠️ Renewal <60d: **GameStory Israel** (2026-08-01, $ TBD) · **Whalo** (2026-08-31, $130K — renewal motion not yet started)
⚠️ Overdue items: Elementor conditional sampling checklist (17d+) · Nexus Mods Agreement task (overdue, blocking) · AT&T escalation to Tal Huberman/Ofer Polivoda (unresolved since Jun 30)

**Total warnings: 13** (matches yesterday's count — no net change, connector outage prevented any harvest/resolution overnight).
