# shortical.com

**Last Updated:** 2026-07-06
**Updated By:** CSA OS (morning-cycle BQ refresh)
**Why Updated:** BQ refresh: event quota overage now 125.7% (up from 117.88%). MTU pricing proposal still overdue.
**Previous:** 2026-06-22 — overage at 117.88%.

---
## Sales Intelligence (BQ refresh — 2026-07-06)
**Health:** Low | **Risk:** High | **Sentiment:** Mixed | **Opp:** shortical.com FY28 Renewal (Stage 1: Pipeline, $76,007)

**Active Risks:**
- Event quota overage now at 125.7% and accelerating.
- Customer actively considering disabling Mixpanel for web tracking.
- MTU pricing proposal is 4+ weeks overdue.

**Open Items:**
- [ ] Deliver MTU pricing proposal to Hai and Yuval — Tal Huberman — due 2026-07-06 (today)
- [ ] Complete overage calculation and event cleanup analysis — Yonah Dorfman — due 2026-07-07
- [ ] Follow up with Igal Rozen post-VIP Event to restart pricing conversation — Yonah Dorfman — due 2026-07-08

Source: BQ (sales_intelligence.account_plans_current), morning-cycle harvest 2026-07-06.

---

## Identity

| Field | Value |
|-------|-------|
| ARR | $71,007 |
| Renewal | 2027-01-31 |
| Industry | Media & Entertainment |
| Segment | SMB |
| Region | EMEA |
| Website | shortical.com |
| AE | TalHuberman (tal.huberman@mixpanel.com) |
| Sales Manager | Ofer Polivoda |
| SFDC | [001Nt00000R8v6UIAR](https://mixpanel.lightning.force.com/lightning/r/Account/001Nt00000R8v6UIAR/view) |
| GDoc | [Account Plan](https://docs.google.com/document/d/10apmh90eoh2eaSNzdWMwgwgfBTafMeBJmd7yovuH2XQ/edit?usp=drivesdk) |

---

## Key Contacts

| Name | Title | Email | Interactions |
|------|-------|-------|-------------|
| hai@shortical.com | — | hai@shortical.com | 2 |
| Igal Rozen | — | Igal Rozen | 1 |
| Gidi Eigenstein | — | Gidi Eigenstein | 1 |
| Yuval Shklarsh | — | Yuval Shklarsh | 1 |
| yuval.s@shortical.com | — | yuval.s@shortical.com | 1 |

---

## Current Health

| Metric | Value |
|--------|-------|
| Overall | Medium |
| VHS Score | 10.0 |
| Adoption Score | 10.0 |
| Sentiment | Neutral |
| Renewal Risk | Medium |

Medium health. While query volume is high, the lack of feature adoption and ongoing data consistency issues pose a risk to the upcoming renewal.

---

## Executive Summary

As of 2026-05-31, shortical.com has an active pipeline deal for their FY 28 Renewal valued at $76,007 ARR with a $5,000 upsell target. Last interaction: 2026-05-31 via Slack Message. Total Calls: 1. Total Emails: 21. The account is exhibiting strong query volume (33K+ queries across 14 users) but zero feature adoption for Session Replay, Copilot, or Alerts. Recent news highlights an Executive Leadership Change in January 2026 ([Shortical Press Room](https://shortical.com/press)) and a strategic pivot toward AI automation. The primary focus is resolving ongoing data consistency issues between their data warehouse and Mixpanel, as well as addressing event volume overages.

---

## Active Risks

- **[ACTIVE]** [2026-06-03] Data consistency discrepancies between the data warehouse and Mixpanel.
- **[ACTIVE]** [2026-06-03] Event volume exceeding the allocated annual budget.
- **[ACTIVE]** [2026-06-03] Zero adoption of advanced features (Session Replay, Copilot, Alerts).
- **[CRITICAL]** [2026-06-10] New website causing event volume explosion — current pricing model unsustainable. Customer considering disabling Mixpanel for web entirely. Churn risk. Escalate pricing conversation to Ofer immediately.


---

## Open Items

- [ ] **[DUE 2026-06-05]** Calculate overages and identify events to clean up to manage budget. (Owner: Yonah Dorfman) — OVERDUE
- [ ] **[DUE 2026-06-10]** Schedule a follow-up meeting to discuss data discrepancies between the data warehouse and Mixpanel. (Owner: Tal Huberman) — OVERDUE
- [x] **[DUE Completed]** Send the presentation slides and documentation on Session Replay to Yuval and Chai. (Owner: Tal Huberman) — completed
- [ ] **[DUE 2026-06-12]** Escalate pricing model to Ofer — propose overage path (cleanup vs tier upgrade) to prevent web tracking disable (Owner: Yonah Dorfman) — URGENT
- [ ] **[DUE 2026-06-12]** Contact Hai/Yuval: discuss new website event volume and pricing options (Owner: Yonah Dorfman) — URGENT


---

## Sales Intelligence

| Field | Value |
|-------|-------|
| Latest Opp | shortical.com - FY 28 Renewal |
| Stage | 1: Pipeline |
| Opp ARR | 76007.0 |

### Mixpanel Telemetry
- **Billing ID:** 5379556
- **Plan usage:** 🔴 117.88% — OVERAGE CONFIRMED (accelerating — was 116.99% Jun 21)
- **VHS:** 10.0 | **Adoption:** 10.0
- **Sentiment:** Mixed | **Renewal Risk:** High
- **Last refreshed:** 2026-06-22

---

## Recent History

**2026-07-06 | DM note [via Slack DM]**
- Shortical signed. Will go over with them soon on how to set up the events.

**2026-06-14 | DM note [via Slack DM]**
- Shortical on Tal, not on Yonah.

**2026-06-10 | Clari call signal [Jun 4 call] — CRITICAL**
- New website launched causing event volume explosion — scale is unsustainable under current pricing
- Customer considering disabling Mixpanel for web tracking entirely
- Churn risk: if they disable web tracking, Mixpanel becomes significantly less valuable
- Immediate action: Yonah to calculate overage, propose cleanup path or tier upgrade
- Escalate to Ofer (Sales Manager) for pricing flexibility conversation
- Follow-up meeting on data discrepancies also overdue (Tal Huberman, due Jun 10)

---
## V/R/G Log (EoD 2026-07-06)
**2026-07-06 | GOAL | Deal signed — event setup walkthrough next**
DM note from Yonah (13:56 IDT) reports the deal is signed and a walkthrough of event setup will follow soon. This lands the same day the overage climbed further (130.65%, up from 125.7% Sunday) and while the MTU pricing proposal to Hai/Yuval was due today (owned by Tal Huberman). Recommend confirming with Tal whether "deal signed" refers to the MTU pricing/tier resolution — if so, the event setup walkthrough is the natural moment to also close out the overage calculation open item (owned by Yonah, due tomorrow Jul 7).
*Source: DM note 2026-07-06 (13:56 IDT) + Mixpanel telemetry 2026-07-06.*
