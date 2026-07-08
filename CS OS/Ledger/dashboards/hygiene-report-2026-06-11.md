# Hygiene Report — 2026-06-11 (Janitor)

**Run:** 06:15 IDT | **Accounts:** 47 | **Book ARR:** ~$3.9M

---

## 1. Freshness Audit

| Status | Count | Accounts |
|--------|-------|---------|
| ✅ Fresh (<7d) | 47 | All accounts — last write: 2026-06-10 (1d ago) |
| ⚠️ Stale (7-14d) | 0 | — |
| 🔴 Critical (>14d) | 0 | — |

Yesterday's cycle patched 18 accounts. All 47 are fresh.

---

## 2. Source Validation

- All 47 accounts have `_context.md` and `_MANIFEST.md` ✅
- No missing files detected ✅
- All Slack channel IDs mapped ✅

---

## 3. Inbox Check

| Item | Age | Status |
|------|-----|--------|
| `2026-05-27-dm-notes.md` | 15 days | ⚠️ Unprocessed — still pending |

---

## 4. Structural Hygiene + Drift

- No local/Notion drifts detected (all resolved Jun 10)
- No malformed context files
- No missing section headers

---

## 5. Notion Health

- Notion MCP available ✅
- No new stale registry entries detected

---

## 6. Renewals <60d + Risk Flags

| Account | Renewal Date | Days | ARR | Stage | Risk |
|---------|-------------|------|-----|-------|------|
| DuxGroup | 2026-06-30 | 19d | $79K | Stage 4: Negotiation | 🔴 CRITICAL — Ghaith Ghazi (rev ops) joined channel Jun 9, asking about timing |
| AT&T Israel | 2026-07-18 | 37d | ~$60K | Stage 4: Negotiation | 🔴 HIGH — Ayelet email unreplied since Jun 10 |
| Elementor | 2026-07-31 | 50d | ~$117K | Active | 🟡 MEDIUM — Ghaith also asking; upsell closed lost Jun 10; tracking checklist unsent |
| MyHeritage | 2026-07-31 | 50d | ~$116K | — | 🟡 MEDIUM — No Itay follow-up yet |
| GameStory | 2026-08-01 | 51d | ~$61K | — | 🟢 LOW |

**Additional Risk Flags:**
- **shortical.com** — Escalation to Ofer due Jun 12 (tomorrow). Churn risk: web deactivation on table.
- **Tabnine** — Ory Yassur MTU churn threat. QBR doc response 13d overdue. $93K at risk.
- **ClickSoftware**, **Talon Cyber Security**, **Altshuler Shaham** — Dark accounts, VHS < 5.0. Monitor.

**New signals overnight:**
- 7 automated Mixpanel "Implementation Issue" emails sent to accounts (FIBI, Boinkers, Loora.AI, RubyLabs, Solaredge, GETT, AutoDS) — monthly batch. Proactively acknowledge with customers where relationship warrants it.
- GETT Jun 18 onsite session canceled by Tal — needs rescheduling.
- Nexus Mods: Dennis Raagaard confirmed upgrade to 24bn tier (Jun 10 email). Deal closing.

---

## Summary

| Check | Status |
|-------|--------|
| Freshness | ✅ All 47 fresh (patched yesterday) |
| Structural | ✅ All 47 complete |
| Inbox | ⚠️ 1 item 15d unprocessed |
| Drift | ✅ None |
| Renewals <60d | ⚠️ 5 renewals; DuxGroup 19d CRITICAL + AT&T 37d HIGH |
| Risk | 🔴 shortical churn; Tabnine MTU churn threat; leadership pressure on renewals |

**Warnings detected → janitor summary posted to Slack.**
