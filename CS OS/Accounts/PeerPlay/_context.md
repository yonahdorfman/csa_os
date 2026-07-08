# PeerPlay

**Last Updated:** 2026-07-06
**Updated By:** CSA OS (morning-cycle BQ refresh + Calendar)
**Why Updated:** Quarterly meeting on calendar today at 15:00 GMT+3 — matches renewal negotiation (Stage 4) action item to confirm Aug 1 contract start. Risks refreshed: data pipeline reliability, 90.8% MTU usage, open P1 compensation.
**Previous:** 2026-06-23 — contract dispute resolved, with legal, $40K upsell expected.

---
## Sales Intelligence (BQ refresh — 2026-07-06)
**Health:** Medium | **Risk:** High | **Sentiment:** Mixed | **Opp:** PeerPlay FY27 Renewal (Stage 4: Negotiation, $90,999)
**Calendar note:** PeerPlay // Mixpanel Quarterly Meeting today at 15:00 GMT+3.

**Active Risks:**
- Data pipeline reliability issues impacting game economy and segmentation.
- High MTU usage (90.8%) approaching plan limits.
- Unresolved compensation items from May 5 P1 incident.

**Open Items:**
- [ ] Confirm Aug 1 contract start + Net 60 terms for renewal — Shavit Ben-Itzhak — due 2026-07-10 (matches today's QBR)
- [ ] Resolve outstanding compensation from May 5 incident — Shavit Ben-Itzhak — due 2026-07-15
- [ ] Follow up on support ticket #5957 (Data Delay on Staging) — Yonah Dorfman — due 2026-07-10

Source: BQ (sales_intelligence.account_plans_current) + Calendar, morning-cycle harvest 2026-07-06.

---

## Identity

| Field | Value |
|-------|-------|
| ARR | $50,673 |
| Renewal | 2026-09-30 |
| Industry | Media & Entertainment |
| Segment | SMB |
| Region | EMEA |
| Website | www.peerplay.com |
| AE | ShavitBen-Itzhak (shavit.ben-itzhak@mixpanel.com) |
| Sales Manager | Ofer Polivoda |
| SFDC | [001Nt000004dx2nIAA](https://mixpanel.lightning.force.com/lightning/r/Account/001Nt000004dx2nIAA/view) |
| GDoc | [Account Plan](https://docs.google.com/document/d/12bE5xJO-Cq_I6AQgSEG9UZRhXMRqFnKp4eifWzcXa-4/edit?usp=drivesdk) |

---

## Key Contacts

| Name | Title | Email | Interactions |
|------|-------|-------|-------------|
| Guy Zamir | CEO/Founder | guy@peerplay.com | 1 |

---

## Current Health

| Metric | Value |
|--------|-------|
| Overall | Medium |
| VHS Score | 7.5 |
| Adoption Score | 10.0 |
| Sentiment | Negative |
| Renewal Risk | High |

Relationship is strained due to a recent P1 data latency incident that impacted their live game operations. While usage remains high, resolving the compensation and technical gaps is critical before the upcoming renewal.

---

## Executive Summary

As of 2026-05-31, PeerPlay has strong team-wide engagement (66K queries across 24 users) but is recovering from a critical data latency incident in early April/May. An FY 27 Renewal opportunity is active in pipeline for $50,673. Last interaction: 2026-05-31 via Slack Message. Total Calls: 1, Total Emails: 30. The company operates as a casual games powerhouse leveraging data-driven insights ([The Org](https://theorg.com/org-chart/peerplay)).

---

## Active Risks

- **[RESOLVED]** [2026-06-22] Contract dispute resolved — now with legal. 40K upsell expected on FY27 renewal. Source: Yonah DM note Jun 22 17:10.
- **[CRITICAL]** [2026-06-21] Building parallel BigQuery pipeline for real-time data — explicitly to reduce dependency on Mixpanel. If live, Mixpanel becomes technically replaceable. "Direct data pipeline to BigQuery running in parallel with Mixpanel" — their words. Source: Clari call Jun 15.
- **[ACTIVE]** [2026-06-21] DAU growth stagnated at ~65K for 2-3 months. Production incidents from Mixpanel data caused incorrect game difficulty/task assignments, harming UX and monetization. Root cause of original credit request. Source: Clari call Jun 15.
- **[ACTIVE]** [2026-06-03] Billing page gap issue
- **[ACTIVE]** [2026-06-03] Cloud Code access issue


---

## Open Items

- [x] ✅ Contract dispute resolved — now with legal. 40K upsell on FY27 renewal. (Jun 22)
- [ ] **[DUE 2026-06-30]** Monitor legal process — confirm signed contract + upsell paperwork (Owner: Shavit Ben-Itzhak)
- [ ] ⚠️ Send concentrated summary of 3 production incidents (timelines + impact) — Guy Zamir — OVERDUE (due ~Jun 17 per call, now Jun 23)
- [ ] ⚠️ Schedule follow-up meeting with Guy Zamir — DUE 2026-06-26
- [ ] **[DUE 2026-06-15]** Investigate billing page gap and share findings (Owner: Yonah Dorfman) — OVERDUE
- [ ] **[DUE 2026-06-15]** Confirm Cloud Code / MCP access (Owner: Yonah Dorfman) — OVERDUE
- [ ] **[DUE 2026-06-30]** Quarterly Meeting and Renewal Discussion (Owner: Shavit Ben-Itzhak) — pending


---

## Sales Intelligence

| Field | Value |
|-------|-------|
| Latest Opp | PeerPlay - FY 27 Renewal |
| Stage | 1: Pipeline |
| Opp ARR | 50673.0 |

### Mixpanel Telemetry
- **Billing ID:** 3972025
- **Plan usage:** ⚠️ 90.8% — approaching limit (was 90.6% Jun 21)
- **VHS:** 7.5 | **Adoption:** 10.0
- **Sentiment:** Negative
- **Last refreshed:** 2026-06-22

---

## Recent History

- 2026-06-15: Contract + risk call (33 min) with Guy Zamir (guy@peerplay.com) + Shavit Ben-Itzhak. Core issues: (1) DAU stagnated at ~65K for 2-3 months; (2) production incidents from Mixpanel data caused incorrect real-time segmentation → wrong game difficulty/task assignments → hurt user experience and monetization; (3) PeerPlay now building a direct BQ pipeline as parallel data source, reducing Mixpanel dependency; (4) Guy seeking contract credits for incident impact and wants to delay new contract start from Aug 1 to Sep 1 or Aug 15. Shavit to confirm feasibility within 2 days. Net 60 payment terms also on the table. Renewal deal is active but structurally at risk. Source: Clari call (https://copilot.clari.com/call/b52f1c4d-87ad-4ee3-bb57-c5ea2217ebf9)

**2026-06-22 | DM note [via Slack DM]**
- Contract has been resolved, now with legal. Will be a 40K upsell.

---
## V/R/G Log (EoD 2026-07-06)
**2026-07-06 | RISK | Quarterly Meeting held — contract terms + support ticket were the agenda**
15:00 QBR occurred per calendar, going in with the goal of confirming Aug 1 contract start + Net 60 terms (Stage 4: Negotiation) and checking status of support ticket #5957 (Data Delay on Staging). No post-call record available yet (Yonah's OOO block started 15:30, right after this call). Confirm outcome tomorrow — this is a high-stakes call given the account's history (BQ parallel-pipeline decoupling risk, prior production incidents) and the fact that Yonah went OOO immediately after.
*Source: Calendar 2026-07-06 + BQ Sales Intelligence 2026-07-06.*
