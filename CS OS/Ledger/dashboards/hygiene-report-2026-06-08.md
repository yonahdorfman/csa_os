# Hygiene Report — 2026-06-08

**Run at:** 07:30 IDT | **CSM:** Yonah Dorfman | **Accounts audited:** 47 folders (46 unique)

---

### Job 1: Context Freshness

✅ Fresh (≤7d): 47 folders — all updated 2026-06-03 (5 days ago)
⚠️ Stale (7-14d): none
🔴 Critical (>14d): none

All account context files were seeded on 2026-06-03. No freshness issues today. **Warning: all contexts will cross into Stale territory on 2026-06-10** if not refreshed.

---

### Job 2: Source Validation

Spot-checked 4 Slack channels and 2 Notion pages.

✅ All sources accessible: 4/4 Slack channels OK
  - AT&T Israel (C04CYT8CSN7) ✅
  - Lemonade (CHE9ZMQM8) ✅
  - SimilarWeb (CLABN6TLK) ✅
  - Stillfront Group AB (C0ANDQGACGK) ✅
✅ Notion: Accounts DB (cfe01032176e455eb849315deff4a68c) ✅ | DuxGroup context page ✅
✅ No broken sources detected in spot-check

Note: Tabnine and Riverside.fm have no external Slack channel IDs in the master index — unable to validate external sources for those accounts (same as prior reports).

---

### Job 3: Inbox

Pending: 1 item

- `2026-05-27-dm-notes.md` — **12 days old** — likely **Whalo** (message: "Check even import errors walo")
  - No `.processed/` folder content exists; item remains unprocessed
  - This item has now appeared in 4 consecutive hygiene reports (Jun 3, 4, 5, 8) without being filed

**Action required:** File the Whalo DM note under Accounts/Whalo/ and investigate import errors on the Whalo project. This is becoming a persistent flag.

---

### Job 4: Structure + Sync

✅ 47/47 folders have both `_context.md` and `_MANIFEST.md`
✅ `overrides.md` — no unfilled `{PLACEHOLDER}` values
✅ `cadence.md` — found at `resources/cadence.md` (not `resources/skills/personal/cadence.md`; path discrepancy with prior report, but file exists and is parseable)

⚠️ **Duplicate Papaya folder — structural drift (persistent, 4th report):**
- Folder 1: `Papaya Global -Formerly Azimo-` (dashes)
- Folder 2: `Papaya Global {Formerly Azimo}` (braces) — both folders contain identical content
- Registry entry: `Papaya Global (Formerly Azimo)` (parentheses)
- **Neither folder name matches the registry key.** Both have renewal: — and health: no assessment.
- Action: Delete one folder, rename surviving folder to `Papaya Global (Formerly Azimo)`, re-seed context.

⚠️ **Empty / uninitialized accounts:**
- **Bookaway** — Renewal: —, Health: no assessment, No health narrative
- **Stillfront Group AB** — Renewal: — (missing from CRM per weekly summary), Health: unknown. Note: Slack active and shows a live, at-risk account
- **Papaya Global (both variants)** — No health data, no contacts, no Slack activity

Drift check: Papaya duplicate persists (2 folders → 1 registry entry). No additional drift detected.

---

### Job 5: Notion

✅ Accounts DB accessible (cfe01032176e455eb849315deff4a68c)
✅ 1/1 spot-checked pages OK (DuxGroup context — renders with full June 3 content)
✅ No stale registry entries detected in spot-check

---

### Job 6: Alerts

**🔴 Renewal <60d:**
- **AT&T Israel** — 2026-06-30 (22 days) | Risk: Medium | Stage 4: Negotiation, $63K | Overdue follow-up with Ayelet Eyal on technical issue adds risk
- **DuxGroup** — 2026-06-30 (22 days) | Risk: Medium | SR upsell CLOSED LOST; open ticket #695833 unresolved; no commercial motion since upsell loss
- **Elementor** — 2026-07-31 (53 days) | Risk: Medium | MTU consumption gap (800K vs 1.5M) is unresolved commercial risk
- **MyHeritage** — 2026-07-31 (53 days) | Risk: Low | $110K in Pipeline; stable
- **GameStory Israel** — 2026-08-01 (54 days) | Risk: Low | Active users, billing dispute to monitor

Note: **Lemonade** renewal date in `_context.md` shows 2026-05-31 (past due), but renewal was confirmed Closed Won May 4 at $80,284 per Slack. Context renewal date needs update.

**⚠️ Critical health / high churn risk:**
- **Tabnine** — Customer explicitly stated will not renew under current MTU terms; FY28 at severe risk ($93K FY27 just closed). Requires immediate commercial resolution.
- **Talon Cyber Security** — Near-zero usage post Palo Alto acquisition; engagement effectively dead; renewal Oct 2026
- **ClickSoftware** — Zero product adoption; renewal Jan 2027; High risk
- **PeerPlay** — P1 data latency incident has strained relationship; renewal Sep 2026
- **PayBox** — Downgrade in process due to budget constraints; renewal Dec 2026
- **Ferryhopper** — Low health; single-user dependency; nested property technical blockers unresolved; renewal Jan 2028
- **Fibi** — Stalled at Stage 1; low usage; renewal Dec 2026
- **Movement-Group** — myIQ security/implementation blocker; renewal Jan 2027
- **Taboola** — Stated intent to reduce Mixpanel usage for publisher dashboards due to API cap; renewal Mar 2027
- **Gamdom** — Critically low usage (223 queries, 6 users) despite onboarding call May 14; renewal Feb 2028
- **Altshuler Shaham** — Critically low usage (181 queries, single user Sari); renewal Apr 2027

**⚠️ Overdue open items (all owned items past due as of 2026-06-08):**

Items >7 days overdue:
- **Nexus Mods** — Resolve Nexus Mods Agreement | DUE 2026-06-01 | **7 days** | Owner: Yonah Dorfman
- **Ferryhopper** — Talk with Max re account strategy | DUE 2026-06-01 | **7 days** | Owner: Yonah Dorfman
- **Base44** — Discuss 'ticket-DP' in weekly sync | DUE 2026-06-01 | **7 days** | Owner: Tal Huberman
- **Taboola** — Conduct Account Review & Sync | DUE 2026-06-01 | **7 days** | Owner: Yonah Dorfman
- **Appcharge LTD** — Resolve Pylon Issue #927 | DUE 2026-06-02 | **6 days** | Owner: Yonah Dorfman
- **StakeMate** — Send power analysis tool to Oliver | DUE 2026-06-02 | **6 days** | Owner: Yonah Dorfman
- **runwayer.com** — Investigate 'Project Stopped Sending Data' alert | DUE 2026-06-02 | **6 days** | Owner: Yonah Dorfman
- **Tabnine** — Respond to Tal's product questions in QBR doc | DUE 2026-06-02 | **6 days** | Owner: Yonah Dorfman

Additional items 1-5 days overdue (due Jun 3–7):
- **Elementor** — Prep for Jun 4 intro meeting (DUE Jun 3, 5d) | Yonah
- **SimilarWeb** — Follow up with Tonya Edelstein on SR/Experiments (DUE Jun 3, 5d) | Tal
- **Simply** — Conduct Quarterly Meeting + advance early renewal (DUE Jun 3, 5d) | Shavit
- **AT&T Israel** — Follow up Ayelet Eyal on missing event fix (DUE Jun 5, 3d) | Yonah
- **Altshuler Shaham** — Send VIP event invitations (DUE Jun 5, 3d) | Tal
- **Altshuler Shaham** — Send MCP documentation (DUE Jun 7, 1d) | Tal
- **Appcharge LTD** — Chase WHI trial approval (DUE Jun 5, 3d) | Tal
- **AutoDS IL** — Resolve invoice issue (DUE Jun 7, 1d) | Tal
- **BlueThrone** — Secure WHC trial approval (DUE Jun 5, 3d) | Shavit
- **boinkers.io** — Resolve Pylon Issue #1812 SLA breach (DUE Jun 5, 3d) | Yonah
- **boinkers.io** — Address Boinkers-prod implementation issue (DUE Jun 7, 1d) | Yonah
- **ClickSoftware** — Identify product champion (DUE Jun 7, 1d) | Shavit
- **Duda** — Send May 25 quarterly meeting recap (DUE Jun 5, 3d) | Yonah/Tal
- **DuxGroup** — Session Replay upsell follow-up (DUE Jun 5, 3d) | Shavit
- **Elementor** — Prepare training session (DUE Jun 6, 2d) | Shavit
- **Ferryhopper** — Consult with Daniel re nested properties (DUE Jun 5, 3d) | Yonah
- **Forex Club** — Schedule proactive check-in with Oleksiy (DUE Jun 5, 3d) | Yonah
- **mbg-digital.com** — Resolve billing/payment verification (DUE Jun 5, 3d) | Tal
- **Nexus Mods** — Complete MCP EU setup documentation (DUE Jun 7, 1d) | Yonah
- **RubyLabs** — Send headless/AI links to Kiryl (DUE Jun 5, 3d) | Shavit
- **RubyLabs** — Follow up on SCIM ticket (DUE Jun 5, 3d) | Yonah
- **shortical.com** — Calculate overages and identify cleanup events (DUE Jun 5, 3d) | Yonah
- **SimilarWeb** — Reschedule canceled June 10 meeting (DUE Jun 5, 3d) | Tal
- **Solaredge** — Send post-meeting follow-up re portal tracking (DUE Jun 5, 3d) | Yonah
- **StakeMate** — Open Session Replay trial (DUE Jun 5, 3d) | Tal
- **Tabnine** — Investigate MTU contract discrepancy (DUE Jun 5, 3d) | Tal
- **Taboola** — Escalate MCP API hourly cap to product/engineering (DUE Jun 5, 3d) | Yonah
- **Talon** — Confirm lunch with Adi (DUE Jun 5, 3d) | Tal
- **Whalo** — Track ticket #701152 resolution (DUE Jun 5, 3d) | Yonah
- **Whalo** — Follow up post-workshop with Mor Lederhendler (DUE Jun 7, 1d) | Yonah

**Total overdue items: 30+ across 20+ accounts. 8 are ≥6 days overdue. Approximately 15 are owned by Yonah Dorfman.**

---

## Summary

| Check | Status | Detail |
|-------|--------|--------|
| Freshness | ✅ Clean | 47/47 fresh (5d old) — warning: approaching stale on Jun 10 |
| Sources | ✅ Clean | All spot-checked OK (4 Slack, 2 Notion) |
| Inbox | ⚠️ Warning | 1 pending item (Whalo DM note, 12d old, 4th consecutive report) |
| Structure | ⚠️ Warning | Papaya duplicate (2 folders, 1 entry); 3 uninitialized accounts |
| Notion | ✅ Clean | DB + 1 context page accessible |
| Renewals <60d | 🔴 Alert | 5 accounts (AT&T Jun 30, DuxGroup Jun 30, Elementor Jul 31, MyHeritage Jul 31, GameStory Aug 1) |
| Critical health | 🔴 Alert | 11 accounts flagged (Tabnine most urgent) |
| Overdue items | 🔴 Alert | 30+ items past due across 20+ accounts; 8 items ≥6 days overdue |

**Total warnings: 20** | Stale: 0 | Broken sources: 0 | Structural: 2 | Renewals <60d: 5 | Critical health: 11 | Overdue items: 30+
