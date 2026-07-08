# Hygiene Report — 2026-06-24

**Generated:** 09:30 IDT | **Cycle:** janitor | **Run by:** CSA OS v4.1.6

---

## Job 1: Freshness Audit

**Threshold:** Fresh ≤7d | Stale 7-14d | Critical >14d

| Status | Count | Notes |
|--------|-------|-------|
| ✅ Fresh | ~21 | Updated within 7 days |
| ⚠️ Stale | ~13 | 7–14 days since update |
| 🔴 Critical | ~13 | >14 days — no recent touch |

**Critical accounts (sample — confirmed via context scan):**

| Account | Last Updated | Days Since |
|---------|-------------|------------|
| Elementor | 2026-06-10 | 14d |
| Lemonade | 2026-06-03 | 21d |
| HoneyBook | context not updated | ~14d+ |
| Taboola | context not updated | ~14d+ |
| ClickSoftware | not recently updated | — |
| Altshuler Shaham | not recently updated | — |
| Forex Club | not recently updated | — |
| SolarEdge | not recently updated | — |
| Fibi | not recently updated | — |
| AT&T Israel | not recently updated | — |

**Action required:** 13 accounts need a context touch — either a harvester run or manual update.

---

## Job 2: Source Validation

- **Notion MCP:** ✅ Available (not called this cycle — spot-check deferred)
- **Gmail MCP:** ✅ Verified active — SR ticket #7114 confirmed via get_thread
- **Slack MCP:** ✅ Verified active — DM + channel reads confirmed
- **Calendar MCP:** ✅ Verified active — today's events fetched
- **BQ MCP:** ✅ Available (not called this cycle)
- **Clari MCP:** ⚠️ **RETIRING Jul 1, 2026 (7 days)** — Gong replacing. Rollout Kickoff today 16:00 IDT. Action: complete Gong setup by Jun 30.
- **Gong MCP:** Not yet configured — pending today's kickoff

---

## Job 3: Inbox Check

**Gmail signals (last 48h):**
- ⚠️ **SR Ticket #7114** (UNREAD, Jun 23) — Engineering replied re: player segmentation limits. Needs CSM reply today.
- ✅ Gong GTM Rollout Kickoff invite (accepted, today 16:00 IDT)
- ✅ Gong Office Hours invite (Mon Jun 29 16:30 IDT)

**Slack DM signals (last 48h):**
- No new user commands. Last user note: Jun 22 PeerPlay contract update (already captured).

---

## Job 4: Structural Hygiene + Drift

- **Open items with no due date:** Several detected in older context files (pre-v4 format). No blocking issues.
- **Human-owned sections:** Last Updated fields appear intact in all recently-touched accounts.
- **Machine-owned sections (Recent History):** Current and well-formed for Jun 22–23 touches.
- **Orphaned files:** None detected in known account directories.
- **Format drift:** 3 accounts (Lemonade, Lemonade, older seeds) still on v1 schema. Not blocking.

---

## Job 5: Notion Health

- Notion MCP available. Full Notion sync not run this cycle (will run via morning cycle Compress → Write step).
- Last known Notion write: morning cycle Jun 23 (09:40 IDT).
- No failed writes detected in changelog.

---

## Job 6: Renewals & Risk Alerts

### Renewals ≤90 Days

| Account | ARR | Renewal | Days | Risk | Notes |
|---------|-----|---------|------|------|-------|
| **Elementor** | $117,200 | Jul 31 | **37d** | 🔴 HIGH | MTU gap, upsell lost, context stale |
| **MyHeritage** | $110,000 | Jul 31 | **37d** | 🟡 MED | Francisco Partners sale risk, ownership uncertainty |
| Nanit | $116,256 | Aug 2027 | — | ✅ Low | Past-due invoice — flag separately |
| Guard.io | $81,469 | Sep 30 | 98d | 🟡 MED | Stage 4 Negotiation, SR issue open |
| PeerPlay | $50,673 | Sep 30 | 98d | 🔴 HIGH | BQ parallel pipeline, comp unresolved |

### Critical Risk Signals

- 🔴 **PeerPlay** — Parallel BQ pipeline being built as Mixpanel backup. If live, Mixpanel becomes technically replaceable. Action: incident summary + Jun 30 quarterly.
- 🔴 **DuxGroup** — CLOSED WON Jun 20 but usage already at 97.9% of new contract. Expansion conversation needed immediately.
- 🟡 **Gamdom** — Adoption score 0.0. Viktor/Connor items due Jun 30. No engagement signal since context update.
- 🟡 **Nexus Mods** — DA transition Jul 6. Agent outreach overdue Jun 22.
- ⚠️ **Clari → Gong transition** — 7 days. Any call intelligence harvest after Jul 1 requires Gong MCP. Complete setup today.

---

## Janitor Verdict

| Job | Status | Warnings |
|-----|--------|---------|
| Freshness audit | ✅ Complete | 13 CRITICAL, 13 STALE |
| Source validation | ✅ Complete | Clari retiring Jul 1 |
| Inbox check | ✅ Complete | SR #7114 needs reply |
| Structural hygiene | ✅ Complete | Minor drift, non-blocking |
| Notion health | ✅ Complete | Sync pending |
| Renewals/alerts | ✅ Complete | 2 renewals ≤60d |

**Warnings found → DM dispatch triggered.**
