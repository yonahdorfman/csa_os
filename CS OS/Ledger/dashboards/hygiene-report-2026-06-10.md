# Hygiene Report — 2026-06-10 (Janitor)

**Run:** 09:19 IDT | **Accounts:** 47 | **Book ARR:** ~$3.9M

---

## 1. Freshness Audit

| Status | Count | Accounts |
|--------|-------|---------|
| ✅ Fresh (<7d) | 0 | — |
| ⚠️ Stale (7-14d) | 47 | All accounts — last local write: 2026-06-03 (7d ago) |
| 🔴 Critical (>14d) | 0 | — |

**Note:** 3 accounts (Gamdom, Nexus Mods, Elementor) have newer Notion data than local cache. Local cache reflects Jun 3; Notion was updated Jun 9. Drift exists — will be resolved by today's compress+write cycle.

---

## 2. Source Validation

- All 47 accounts have both `_context.md` and `_MANIFEST.md` ✅
- No missing files detected ✅
- All Slack channel IDs mapped in overrides ✅

---

## 3. Inbox Check

| Item | Age | Status |
|------|-----|--------|
| `2026-05-27-dm-notes.md` | 14 days | ⚠️ Unprocessed — review and file |

**Action required:** `CS OS/Inbox/2026-05-27-dm-notes.md` — 14 days old, never filed.

---

## 4. Structural Hygiene + Drift

- **3 local/Notion drifts detected:**
  - Gamdom: Notion updated 2026-06-09, local shows 2026-06-03
  - Nexus Mods: Notion updated 2026-06-09, local shows 2026-06-03
  - Elementor: Notion updated 2026-06-09, local shows 2026-06-03
- No malformed context files detected
- No missing section headers detected
- V/R/G Log entries: present in all accounts ✅

---

## 5. Notion Health

- Notion MCP available ✅
- 3 pages with known drift (to be synced in morning cycle)
- No stub/empty pages detected

---

## 6. Renewals <60 Days + Risk Flags

| Account | Renewal Date | Days | ARR | Stage | Risk |
|---------|-------------|------|-----|-------|------|
| DuxGroup | 2026-06-30 | 20d | $79K | Stage 4: Negotiation | 🔴 HIGH — SR stalled since April; must close this week |
| AT&T Israel | 2026-07-18 | 38d | ~$87K | Stage 4: Negotiation | 🟡 MEDIUM — Email in this morning; upsell active |
| Elementor | 2026-07-31 | 51d | ~$117K | Active | 🟡 MEDIUM — Jun 8 call flagged tracking impl issues |
| MyHeritage | 2026-07-31 | 51d | ~$116K | — | 🟢 LOW — No recent signals |
| GameStory | 2026-08-01 | 52d | ~$61K | — | 🟢 LOW — No recent signals |

**Additional Risk Flags:**
- **shortical.com** — No upcoming renewal but HIGH churn risk: Jun 4 call revealed event volume explosion from new website; customer considering disabling Mixpanel for web. Current pricing model unsustainable for their new scale. Immediate action needed.
- **ClickSoftware**, **Talon Cyber Security**, **Altshuler Shaham** — BQ shows VHS < 5.0 and adoption 0; effectively dark accounts. Monitor.
- **Lemonade** — NPS detractors flagged in Slack: alice.saugrain (score 3), kati.withnell, matan.rahav.

---

## Summary

| Check | Status |
|-------|--------|
| Freshness | ⚠️ All accounts borderline stale (7d). Will be resolved today. |
| Structural | ✅ All 47 complete |
| Inbox | ⚠️ 1 item 14d unprocessed |
| Drift | ⚠️ 3 accounts local/Notion mismatch |
| Renewals <60d | ⚠️ 5 renewals, 1 critical (DuxGroup 20d) |
| Risk | 🔴 shortical churn risk; DuxGroup renewal urgency |

**Warnings detected → janitor summary posted to Slack.**
