# boinkers.io BY Acid Labs

**Last Updated:** 2026-07-07
**Updated By:** CSA OS (Gmail catch-up — 14-day sweep, requested by Yonah)
**Why Updated:** "Can't edit filled-in range" bug now tracked as ticket #8106 — workaround still owed to customer, automated follow-up unanswered since Jul 4.
**Previous:** 2026-06-10 — external channel scan (Export API dedup bug, lookup table broken).

---

## Identity

| Field | Value |
|-------|-------|
| ARR | $148,813 |
| Renewal | 2026-12-31 |
| Industry | Other |
| Segment | SMB |
| Region | EMEA |
| Website | boinkers.io |
| AE | OferPolivoda (ofer.polivoda@mixpanel.com) |
| Sales Manager | Ghaith Ghazi |
| SFDC | [001Nt00000GAdi2IAD](https://mixpanel.lightning.force.com/lightning/r/Account/001Nt00000GAdi2IAD/view) |
| GDoc | [Account Plan](https://docs.google.com/document/d/1ukCDLwzC07VwU0iGIao0hb_9MbNEKRz1UqLd_Y1EEA4/edit?usp=drivesdk) |

---

## Key Contacts

| Name | Title | Email | Interactions |
|------|-------|-------|-------------|
| maor@acid.fun | — | maor@acid.fun | 3 |

---

## Current Health

| Metric | Value |
|--------|-------|
| Overall | Medium |
| VHS Score | 8.4 |
| Adoption Score | 10.0 |
| Sentiment | Mixed |
| Renewal Risk | Medium |

Medium. While overall query volume is exceptionally high and Copilot is being adopted, the account is at risk due to extreme single-user dependency, unresolved implementation issues on Boinkers-prod, and recent support SLA breaches.

---

## Executive Summary

As of 2026-05-31, boinkers.io has an active pipeline deal for their FY 27 Renewal at $148,813 ARR. Last interaction: 2026-05-31 via Slack Message. Total Calls: 0. Total Emails: 0. The account exhibits massive query volume (147K+ queries), but usage is highly concentrated in a single user (alexander.mazyarik). Acid Labs recently raised $8M in seed funding ([The Defiant](https://thedefiant.io/news/defi/acid-labs-raises-8m-from-a16z-speedrun-and-nfx-to-scale-instant-social-games-on-chatapps)) and reached 25M users by late 2025 ([TON.org](https://ton.org/en/blog/games-worth-your-attention-boinkers)). Immediate attention is required to resolve an SLA breach on a Pylon issue and an ongoing Boinkers-prod implementation issue.

---

## Active Risks

- **[ACTIVE]** [2026-06-03] Extreme single-user concentration (alexander.mazyarik drives 65% of queries).
- **[ACTIVE]** [2026-06-03] Unresolved Boinkers-prod implementation issue open since May 10.
- **[ACTIVE]** [2026-06-03] SLA breaches on Pylon Issues #1812 and #707.
- **[ACTIVE]** [2026-06-03] Zero adoption of Session Replay despite it being included in the plan.
- **[ACTIVE]** [2026-06-10] Export API deduplication gap — financial reporting impact. 58 users had events tripled (372 TON via API vs 124 TON in UI). Yonah replied Jun 8: workaround provided (group by distinct_id+insert_id client-side), product feedback submitted and tagged Scott Cohen + AK Krivitzky. Underlying API parity gap remains a product-level issue.
- **[ACTIVE]** [2026-06-10] Jun 4: Lookup table broken — custom property not appearing in reports. Unreplied.


---

## Open Items

- [ ] **[DUE 2026-06-05]** Resolve Pylon Issue #1812 (SLA breach) regarding Dashboard Feature Request (Owner: Support/Yonah Dorfman) — pending
- [ ] **[DUE 2026-06-07]** Address Boinkers-prod implementation issue (open since May 10) (Owner: Yonah Dorfman) — pending
- [ ] **[DUE 2026-06-15]** Onboard maya@acid.fun and maor@acid.fun as secondary champions to ease concentration risk (Owner: Ofer Polivoda) — pending
- [x] **[DONE 2026-06-08]** Export API duplicate events — acknowledged, workaround provided, product feedback tagged to Scott Cohen + AK Krivitzky (Owner: Yonah Dorfman)
- [ ] **[DUE 2026-06-11]** Investigate Jun 4 lookup table broken + custom property not in reports (Owner: Yonah Dorfman) — pending
- [ ] **[OVERDUE — deadline passed 2026-07-01]** EU data residency: *pokergram-prod* still routing ~56M events/month to US endpoint. Flagged Jun 21 in `#ext-mixpanel-acid`, asked if fixable by Jul 1 — no confirmation. (Owner: Yonah Dorfman) — needs urgent follow-up
- [ ] **[OPEN since late June]** Fresh wave of UI bugs reported by the single power-user (maorf): can't edit a filled-in range (Jun 29), property selection gets stuck after picking one (Jun 28), "top 50" random sample table doesn't actually show top 50 (Jun 29), and general difficulty configuring lookup tables (Jun 30). None confirmed resolved — pattern worth a proactive check-in given single-user concentration risk already flagged. (Owner: Yonah Dorfman)
- [x] **[DONE 2026-07-06]** The "can't edit a filled-in range" bug (support ticket #8106) is resolved and closed. Support shared a workaround Jul 1, Yonah confirmed Jul 6 the workaround was communicated to the customer, and support (Dorothy) closed the ticket same day. (Owner: Yonah Dorfman)


---

## Sales Intelligence

| Field | Value |
|-------|-------|
| Latest Opp | boinkers.io BY Acid Labs - FY 27 Renewal |
| Stage | 1: Pipeline |
| Opp ARR | 148813.0 |

---

## Recent History

**2026-07-02 | External channel scan — EU residency deadline missed + bug cluster**
- Jun 21: EU data residency flag for pokergram-prod (~56M events) — no confirmation by Jul 1 deadline.
- Jun 28-30: maorf (the account's single power-user, already flagged as a concentration risk) reported four separate UI bugs/friction points in under a week (property selection stuck, range editing broken, "top 50" sample not matching, lookup table config difficulty). Given this is a single-threaded relationship, a cluster of unresolved bugs like this raises real churn/frustration risk — worth a proactive outreach beyond just answering each ticket.
- *Source: Slack (#ext-mixpanel-acid), harvest-catchup 2026-07-02.*

**2026-06-10 | External channel scan**
- Jun 8: Export API deduplication gap — Yonah replied same day. Acknowledged the issue, provided workaround (deduplicate on distinct_id+insert_id client-side), submitted product feedback. Scott Cohen + AK Krivitzky tagged for product awareness. Underlying API parity gap remains open at product level.
- Jun 4: Lookup table broken — custom property not appearing in reports. Still unreplied.
- Both issues require immediate outreach; financial impact of Export API bug is the top priority.

**2026-07-21 | Slack full catch-up (#at-boinkers-acid, #ext-mixpanel-acid, DMs for Maor/AK Krivitzky/Asaf Huga/Guy Bauer)**
- RESOLVED TODAY: support (Ilhan Abdalle) confirmed the drag-and-drop Filter/Breakdown bug (ticket #9065, reported Jul 5 by maorf) is fixed as of today (Jul 21) — asked customer to confirm on their side. This closes out a functionality gap the power user said they used "DAILY."
- Two other Pylon issues remain open and need status checks: #10338 (critical bug) and #10835 (Appsflyer_ID column inconsistency in _MASTER_EVENT view).
- Customer story momentum: Boinkers' Mixpanel Headless usage (programmatic custom properties/cohorts) is being developed into a customer story with Nick Lin/Ella Sandovski — EMEA team flagged it for spotlighting.
- Single-user concentration risk persists (alexander.mazyarik ~75% of real-user queries); recurring monthly "Implementation Issue" notification for Boinkers-prod fired again, still unresolved.
