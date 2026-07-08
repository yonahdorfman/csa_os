# Hygiene Report — 2026-06-05

**Run at:** 07:30 IDT | **CSM:** Yonah Dorfman | **Accounts audited:** 47 folders (46 unique)

---

### Job 1: Context Freshness

✅ Fresh (≤7d): 47 folders — all updated 2026-06-03 (2 days ago)
⚠️ Stale (7-14d): none
🔴 Critical (>14d): none

All account context files were seeded on 2026-06-03. No freshness issues.

---

### Job 2: Source Validation

Spot-checked 3 Slack channels and 3 Notion pages.

✅ All sources accessible: 3/3 Slack channels OK (AT&T Israel C04CYT8CSN7, Elementor C079QRB9WDT, Forex Club CHE6XPFAP)
✅ Notion: Accounts DB (cfe01032176e455eb849315deff4a68c) ✅ | BlueThrone context ✅ | RubyLabs context ✅
✅ No broken sources detected in spot-check

Note: Tabnine and Riverside.fm have no external Slack channel IDs in the master index — unable to validate external sources for those accounts.

---

### Job 3: Inbox

Pending: 1 item

- `2026-05-27-dm-notes.md` (9 days old) — likely **Whalo** (message: "Check even import errors walo")
  - No `.processed/` folder exists yet; all inbox items are unprocessed

**Action required:** File the Whalo DM note under Accounts/Whalo/ and check for any open import errors on the Whalo project.

---

### Job 4: Structure + Sync

✅ 47/47 folders have both `_context.md` and `_MANIFEST.md`
✅ `overrides.md` — no unfilled `{PLACEHOLDER}` values

⚠️ **Missing file:** `cadence.md` not found at `resources/skills/personal/cadence.md` — file does not exist (empty output, not parseable)

⚠️ **Duplicate Papaya folder — structural drift:**
- Folder 1: `Papaya Global -Formerly Azimo-` (dashes)
- Folder 2: `Papaya Global {Formerly Azimo}` (braces)
- Registry entry: `Papaya Global (Formerly Azimo)` (parentheses)
- **Neither folder name matches the registry key.** Both folders have no data populated (renewal: —, health: no assessment available).
- Action: Delete one folder, rename the other to match registry key, re-seed context.

⚠️ **Empty accounts (no context data):** The following have renewal: — and health: unknown — likely not yet seeded or inactive:
- Bookaway
- Stillfront Group AB
- Both Papaya Global variants

Drift check: 2 drifted (Papaya duplicates) — run `/reconcile fix` to resolve.

---

### Job 5: Notion

✅ Accounts DB accessible (cfe01032176e455eb849315deff4a68c)
✅ 2/2 spot-checked pages OK (BlueThrone context, RubyLabs context — both render with full content)
✅ No stale registry entries detected in spot-check

---

### Job 6: Alerts

**🔴 Renewal PAST DUE:**
- **Lemonade** — renewal was 2026-05-31 (5 days ago) | Renewal Risk: Low | Note: recently closed renewal, likely admin lag

**🔴 Renewal <60d:**
- **AT&T Israel** — 2026-06-30 (25 days) | Risk: Medium | Stage 4: Negotiation, $63K
- **DuxGroup** — 2026-06-30 (25 days) | Risk: Medium | Active upsell stalled post-demo
- **Elementor** — 2026-07-31 (56 days) | Risk: Medium | MTU overuse gap (800K vs 1.5M)
- **MyHeritage** — 2026-07-31 (56 days) | Risk: Low | $110K in Pipeline
- **GameStory Israel** — 2026-08-01 (57 days) | Risk: Low | Stable, billing dispute to monitor

**⚠️ Critical health / high churn risk:**
- **Tabnine** — customer explicitly stated will not renew; MTU pricing dispute ($93K FY27 just closed, FY28 at risk)
- **Talon Cyber Security** — near-zero usage post Palo Alto acquisition; severe renewal risk Oct 2026
- **ClickSoftware** — zero product adoption; renewal Jan 2027; High risk
- **PeerPlay** — P1 data latency incident; strained relationship; renewal Sep 2026
- **PayBox** — downgrade in process; High risk; renewal Dec 2026
- **Ferryhopper** — Low health; single-user dependency; technical blockers (nested properties); renewal Jan 2028
- **Fibi** — stalled at Stage 1; low usage; High risk; renewal Dec 2026
- **Movement-Group** — myIQ security blocker; High risk; renewal Jan 2027
- **Taboola** — stated intent to reduce Mixpanel usage for publisher dashboards; renewal Mar 2027
- **Gamdom** — critically low usage (223 queries, 6 users) despite recent onboarding; renewal Feb 2028
- **Altshuler Shaham** — critically low usage (181 queries, single user Sari); renewal Apr 2027

**⚠️ Overdue open items (past due date):**
- **Nexus Mods** — "Resolve overdue Nexus Mods Agreement" | DUE 2026-06-01 | **4 days overdue** | Owner: Yonah Dorfman
- **Tabnine** — "Respond to Tal's product questions in the QBR doc" | DUE 2026-06-02 | **3 days overdue** | Owner: Yonah Dorfman

**⏰ Due today (2026-06-05):**
- **AT&T Israel** — Follow up with Ayelet Eyal on missing event issue (Owner: Yonah)
- **DuxGroup** — Follow up on Session Replay upsell post-demo (Owner: Shavit)
- **Tabnine** — Investigate MTU contract discrepancy (Owner: Tal)

---

## Summary

| Check | Status | Detail |
|-------|--------|--------|
| Freshness | ✅ Clean | 47/47 fresh (2d old) |
| Sources | ✅ Clean | All spot-checked OK |
| Inbox | ⚠️ Warning | 1 pending item (Whalo, 9d old) |
| Structure | ⚠️ Warning | cadence.md missing; Papaya duplicate (2 folders, 1 entry) |
| Notion | ✅ Clean | DB + 2 pages accessible |
| Renewals <60d | 🔴 Alert | 6 accounts (1 past due: Lemonade) |
| Critical health | 🔴 Alert | 11 accounts flagged |
| Overdue items | ⚠️ Warning | 2 items past due (Nexus Mods, Tabnine) |

**Total warnings: 16** | Stale: 0 | Broken sources: 0 | Structural: 2 | Renewals <60d: 6 | Critical health: 11 | Overdue items: 2
