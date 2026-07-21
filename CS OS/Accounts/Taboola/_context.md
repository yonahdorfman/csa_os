# Taboola

**Last Updated:** 2026-07-07
**Updated By:** CSA OS (Gmail catch-up — 14-day sweep, requested by Yonah)
**Why Updated:** New billing risk — invoice #72002 flagged 30+ days past due.
**Previous:** 2026-06-10 — Clari call Jun 7 (A/B only usage; Deeper Dive stalled).

---

## Identity

| Field | Value |
|-------|-------|
| ARR | $87,699 |
| Renewal | 2027-03-31 |
| Industry | B2B |
| Segment | ENT |
| Region | EMEA |
| Website | taboola.com |
| AE | ShavitBen-Itzhak (shavit.ben-itzhak@mixpanel.com) |
| Sales Manager | Ofer Polivoda |
| SFDC | [0013q00002EuqNcAAJ](https://mixpanel.lightning.force.com/lightning/r/Account/0013q00002EuqNcAAJ/view) |
| GDoc | [Account Plan](https://docs.google.com/document/d/17nE28fauhahZv0jirkWh35geBEk6mHjtFzLKqJ5Pn5E/edit?usp=drivesdk) |

---

## Key Contacts

_No contacts data available._

---

## Current Health

| Metric | Value |
|--------|-------|
| Overall | Low |
| VHS Score | 7.5 |
| Adoption Score | 5.0 |
| Sentiment | Mixed |
| Renewal Risk | High |

At Risk. Despite a recent rebound in query volume, the customer is actively building internal workarounds (JQL/custom dashboards) to bypass Mixpanel's MCP API limitations.

---

## Executive Summary

As of 2026-05-31, Taboola is in post-renewal mode with an active pipeline deal 'Taboola - FY 28 Renewal' for $95,699 ARR. Last interaction: 2026-05-31 via Slack Message. Total Calls: 1. Total Emails: 18. Usage has rebounded to 15.2K queries, but there is an active risk regarding their stated intent to reduce Mixpanel usage for publisher dashboards due to MCP API limitations. Taboola recently reported strong Q1 2026 results with $466.4M in revenue ([Alphastreet](https://www.alphastreet.com/news/taboola-com-q1-2026-earnings-report/)).

---

## Active Risks

- **[ACTIVE]** [2026-06-03] Stated intent to reduce Mixpanel usage
- **[ACTIVE]** [2026-06-03] MCP API hourly request limits (60/hr) blocking customer workflows
- **[ACTIVE]** [2026-06-03] Statisfy flagged as a competing tool gaining traction
- **[ACTIVE]** [2026-06-10] Jun 7 call: customer using Mixpanel for A/B testing only — "basic" usage. Deeper Dive pitch stalled. Risk of downgrade or churn at Mar 2027 renewal.
- **[ACTIVE]** [2026-07-07] AR flagged invoice #72002 as 30+ days past due (Jun 28 email, olga.kiynovsky@taboola.com CC'd on the AR thread). Billing risk stacking on top of the usage/downgrade risk already tracked — worth checking with AR/Ofer whether this is a data lag or a genuine payment holdup, especially given the account is already flagged as reducing Mixpanel usage.


---

## Open Items

- [ ] **[DUE 2026-06-01]** Conduct Account Review & Sync (Owner: Yonah Dorfman) — OVERDUE
- [ ] **[DUE 2026-06-05]** Escalate MCP API hourly cap limitation to product/engineering (Owner: Yonah Dorfman) — OVERDUE
- [ ] **[DUE 2026-06-17]** Schedule Deeper Dive session — expand usage beyond A/B testing (Owner: Yonah Dorfman) — pending

---

## Sales Intelligence

| Field | Value |
|-------|-------|
| Latest Opp | Taboola - FY 28 Renewal |
| Stage | 1: Pipeline |
| Opp ARR | 95699.0 |

---

## Recent History

**2026-06-10 | Clari call signal [Jun 7 call]**
- Customer confirmed using Mixpanel primarily for A/B testing only — "basic" use case
- Deeper Dive pitch stalled — customer not engaged on expanding use cases
- At-risk of downgrade or churn at Mar 2027 renewal if usage doesn't expand
- MCP API rate limit (60/hr) blocking publisher dashboard workflows — still unescalated
- Statisfy gaining ground as an alternative
- Action: schedule Deeper Dive session; escalate API cap to product/engineering

**2026-06-10 | External channel scan [Jun 2 message — RESOLVED]**
- Customer reported identify-only events on Jun 2. Was a Mixpanel platform-wide ingestion delay incident, not a customer-side SDK issue.
- Yonah + support replied same day. Support provided full incident updates (Ross, Ilhan, Miles). Ticket #3539 closed. All data intact, no tracking regression.

**2026-07-21 | Slack full catch-up (#at-taboola, #mixpanel-taboola, group DMs x2, #mixpanel-taboola-news, DM Aviv Sinai)**
- Renewal figure/stage discrepancy worth reconciling in SFDC: today's internal briefing shows $189,650 at Stage 4: Negotiation (Oct 31 close) vs. the previously tracked $95,699 at Stage 1.
- Official Mixpanel MCP adoption continues at modest scale (52 queries this week, mostly ohad.l@taboola.com) despite the account historically relying on its own in-house MCP client — a positive shift worth watching.
- Feature usage (Copilot, Replays, Alerts, Value Moments) dropped to zero this week from ~45 Value Moments the prior week.
- No new raw customer messages in any of the scanned channels/DMs this week; all recent activity is automated weekly digests. Long-standing MCP API hourly cap issue and Lior S. VIP invite remain open/overdue.
