# Hygiene Report — 2026-06-03
**Generated:** 07:30 IDT | **CSM:** Yonah Dorfman | **Version:** 4.1.6

---

### Job 1: Context Freshness

✅ Fresh (≤7d): 47 accounts
⚠️ Stale (7-14d): none
🔴 Critical (>14d): none

All 47 account _context.md files carry Last Updated: 2026-06-03.

---

### Job 2: Source Validation

✅ Slack spot-checks (3/3 accessible):
- #at-lemonade (CHE9ZMQM8) ✅
- #at-similarweb (CLABN6TLK) ✅
- #at-bluethrone (C0AN6L3TZQU) ✅

✅ Notion Accounts DB (cfe01032176e455eb849315deff4a68c) accessible
✅ Taboola Context page (374e0ba9-2562-81d9-a311-dd056a3b95d6) renders OK

⚠️ Access issues:
- BlueThrone: Context page ID 374e0ba9-2562-8159-b5e4-e734b9c4db66 returned unexpected page ("Morning Briefing — June 3, 2026") — registry entry may be stale. Recommend re-running setup or manually verifying.

---

### Job 3: Inbox

✅ Pending: 0 items — Inbox is clear.

---

### Job 4: Structure + Sync

✅ 47/47 accounts have complete file sets (_context.md + _MANIFEST.md)

⚠️ Duplicate/drift — Papaya Global folders:
- Local: "Papaya Global -Formerly Azimo-" (dashes)
- Local: "Papaya Global {Formerly Azimo}" (curly braces)
- Registry: "Papaya Global (Formerly Azimo)" (parentheses)
Neither local folder name matches the registry entry. One of these is orphaned. Recommend deleting the duplicate and renaming to match the registry.

⚠️ Registry count (46) vs local folders (47): the extra folder is the duplicate Papaya entry above.

Drift check:
✅ 46 accounts in sync (local context matches Notion seeding date 2026-06-03)
⚠️ 1 drift — Papaya Global (duplicate folder, ambiguous registry match) — run `/reconcile fix` to resolve

---

### Job 5: Notion

✅ Accounts DB accessible (cfe01032176e455eb849315deff4a68c)
✅ 1/2 spot-checked pages OK (Taboola context renders correctly)
⚠️ 1 stale registry entry: BlueThrone context page ID returned wrong page — may need refresh

---

### Job 6: Alerts

🔴 Renewal PAST (overdue):
- Lemonade — 2026-05-31 (3 days past) | Risk: Low | ARR: ~$70K | Stage 4: Negotiation active

🔴 Renewal <60d:
- AT&T Israel — 2026-06-30 (27 days) | Risk: Medium | ARR: $63K
- DuxGroup — 2026-06-30 (27 days) | Risk: Medium | ARR: $79K (renewal + upsell in negotiation)
- Elementor — 2026-07-31 (58 days) | Risk: Medium | ARR: $117K
- MyHeritage Ltd. — 2026-07-31 (58 days) | Risk: Low | ARR: $110K

⚠️ High renewal risk (not yet <60d):
- Talon Cyber Security — 2026-10-31 | Risk: High | near-zero usage, Palo Alto acquisition
- PeerPlay — 2026-09-30 | Risk: High | P1 incident, strained relationship
- Fibi — 2026-12-31 | Risk: High
- PayBox — 2026-12-31 | Risk: High | active downgrade in process
- Tabnine — 2027-06-01 | Risk: High | threatening churn over pricing
- ClickSoftware — 2027-01-01 | Risk: High | near-zero usage
- Taboola — 2027-03-31 | Risk: High | MCP API limitations risk
- Movement-Group — 2027-01-31 | Risk: High
- Altshuler Shaham — 2027-04-01 | Risk: High | single-user dependency
- Ferryhopper — 2028-01-31 | Risk: High
- Gamdom — 2028-02-01 | Risk: High

⚠️ Overdue items (past 2026-06-03) — 9 items across 9 accounts:

| Account | Item | Was Due | Owner |
|---------|------|---------|-------|
| Base44 | Discuss 'ticket - DP' in weekly sync | 2026-06-01 | Tal Huberman |
| Ferryhopper | Talk with Max re account strategy | 2026-06-01 | Yonah Dorfman |
| Nexus Mods | Resolve Nexus Mods Agreement | 2026-06-01 | Yonah Dorfman |
| Taboola | Conduct Account Review & Sync | 2026-06-01 | Yonah Dorfman |
| Appcharge LTD | Resolve Pylon Issue #927 SLA breach | 2026-06-02 | Yonah Dorfman |
| HoneyBook | Get SR trial approval from Ofer Polivoda | 2026-06-02 | Tal Huberman |
| StakeMate | Send power analysis tool to Oliver | 2026-06-02 | Yonah Dorfman |
| Tabnine | Respond to Tal's product questions in QBR doc | 2026-06-02 | Yonah Dorfman |
| runwayer.com | Investigate 'Project Stopped Sending Data' alert | 2026-06-02 | Yonah Dorfman |

---

## Summary

| Metric | Value |
|--------|-------|
| Accounts audited | 47 |
| Fresh contexts | 47/47 |
| Slack channels accessible | 3/3 (spot-check) |
| Broken sources | 1 suspect (BlueThrone Notion ID) |
| Inbox pending | 0 |
| Structural issues | 1 (Papaya duplicate folder) |
| Accounts in sync | 46 |
| Drifted | 1 |
| Renewals past due | 1 (Lemonade) |
| Renewals <60d | 4 |
| High-risk accounts | 11 |
| Overdue items | 9 |
| **Total warnings** | **15** |
