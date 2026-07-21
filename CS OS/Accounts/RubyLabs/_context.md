# RubyLabs

**Last Updated:** 2026-07-15
**Updated By:** CSA OS (morning cycle — Slack + Gong harvest)
**Why Updated:** Two new implementation-issue alerts this week (AI project, JobAssist project) need owners; Gong call re: AI skill rollout.
**Previous:** 2026-06-10 — external channel scan (EU/US region transfer question, unreplied).

---

## Machine Patch — 2026-07-15 (Morning Cycle)
### Two new implementation-issue alerts this week — need owners
- Automated Mixpanel implementation-issue alerts fired for RubyLabs' "AI" project (2026-07-10, first notification) and "JobAssist" project (2026-07-07, first notification) — both could affect data quality and neither has a confirmed owner yet.
- 2026-07-08 Gong call (7 min, Shavit) tied to the FY28 Renewal ($189,650): RubyLabs' team is building an AI skill for personalized customer offers, still deciding on rollout timing, and has concerns about Claude content-quality standards — wants Mixpanel's input.
- Carried open items still unresolved: myIQ implementation issue, EU data-residency confirmation, and Shavit's headless/AI links for Kiryl (originally overdue 5+ weeks as of last week — now likely 6+ weeks).
- Source: Slack #at-rubylabs + Gong call (call_id 5189022912725106472), harvested via morning-cycle 2026-07-15.

---

## Identity

| Field | Value |
|-------|-------|
| ARR | $174,650 |
| Renewal | 2027-10-31 |
| Industry | Media & Entertainment |
| Segment | SMB |
| Region | EMEA |
| Website | www.rubylabs.io |
| AE | ShavitBen-Itzhak (shavit.ben-itzhak@mixpanel.com) |
| Sales Manager | Ofer Polivoda |
| SFDC | [001Nt00000N9GMsIAN](https://mixpanel.lightning.force.com/lightning/r/Account/001Nt00000N9GMsIAN/view) |
| GDoc | [Account Plan](https://docs.google.com/document/d/1Bsn_9E2oV3LV8IfDa9Qinetb8HWj1xs3k-S49GiJUtw/edit?usp=drivesdk) |

---

## Key Contacts

| Name | Title | Email | Interactions |
|------|-------|-------|-------------|
| k.panin@rubylabs.com | — | k.panin@rubylabs.com | 1 |
| g.lazarev@rubylabs.com | — | g.lazarev@rubylabs.com | 1 |
| p.stepien@rubylabs.com | — | p.stepien@rubylabs.com | 1 |
| a.mehmood@rubylabs.com | — | a.mehmood@rubylabs.com | 1 |
| k.tymchenko@rubylabs.com | — | k.tymchenko@rubylabs.com | 1 |

---

## Current Health

| Metric | Value |
|--------|-------|
| Overall | High |
| VHS Score | 10.0 |
| Adoption Score | 10.0 |
| Sentiment | Mixed |
| Renewal Risk | Low |

Account health is very high. The recent Session Replay upsell was successful, usage is massive, and the customer is actively exploring advanced features like Mixpanel Headless and AI Agent.

---

## Executive Summary

As of 2026-05-31, RubyLabs is a highly engaged, top-tier account with exceptional product usage. A $34,650 Session Replay upsell was Closed Won on May 3, 2026, and the FY28 Renewal ($189,650) is currently in the pipeline. Last interaction: 2026-05-31 via Slack Message. Total Calls Logged: 1. Total Emails Logged: 14. The account ranks highly on the global MCP leaderboard with over 282K queries across 130+ users. The team recently held a Strategic Quarterly Meeting on May 27, 2026, focusing on Mixpanel's new AI capabilities and Mixpanel Headless.

---

## Active Risks

- **[ACTIVE]** [2026-06-03] SCIM integration limitations with Google Workspace


---

## Open Items

- [ ] **[DUE 2026-06-05]** Send links and examples for Mixpanel headless and AI capabilities to Kiryl via chat (Owner: Shavit Ben-Itzhak) — pending
- [ ] **[DUE 2026-06-15]** Schedule a dedicated AI session with Kiryl's team (Owner: Shavit Ben-Itzhak) — pending
- [ ] **[DUE 2026-06-05]** Follow up on SCIM ticket resolution (Owner: Support / Yonah) — pending
- [ ] **[OVERDUE — deadline passed 2026-07-01]** EU data residency: multiple projects still on US endpoint — *Hint* (~263M events, highest volume), *getQR* (~14M), plus smaller ones. Flagged Jun 21 in `#mixpanel-rubylabs`, asked who owns this — no reply visible. (Owner: Yonah Dorfman) — needs urgent follow-up, largest EU-residency exposure across the book



---

## Sales Intelligence

| Field | Value |
|-------|-------|
| Latest Opp | RubyLabs - FY 28 Renewal |
| Stage | 1: Pipeline |
| Opp ARR | 189650.0 |

---

## Recent History

**2026-07-21 | Slack full catch-up — new support ticket + two implementation-issue alerts still need owners**
- New Pylon issue #10505 ("Timezone handling for user event timestamps") received new customer messages Jul 18–19 — needs triage.
- Two Mixpanel implementation-issue alerts fired this month remain unassigned: "AI" project (Jul 10) and "JobAssist" project (Jul 7).
- Also flagged in #mixpanel-rubylabs this week: event-ingestion drop reported Jul 11 (~7:30am BST, resolved same day per thread), and a UI bug (Jul 10) with cut-off dropdowns blocking chart-type selection (4 replies, still open as of Jul 11).
- Flagship-level usage continues (271K+ queries, 130+ users, 11,233 MCP calls this week) — no churn signal, purely an operational-follow-up gap.
- myIQ implementation issue and EU data-residency confirmation remain open carryovers from prior weeks.

**2026-07-02 | External channel scan — EU data residency deadline missed (largest exposure in book) + MCP engagement signal**
- Jun 21: Yonah flagged *Hint* (~263M events) and *getQR* (~14M) plus smaller projects still routing to US endpoint before Jul 1 — no reply visible, deadline now passed.
- Jun 28: Yonah asked the channel how the team uses Mixpanel's MCP/API day-to-day — no reply yet, but ties into the Jul 1 Weekly Pulse showing 38,144 MCP tool calls in 90 days from 15-27 people/week — genuinely agentic usage, worth a dedicated follow-up call given the AI/MCP engagement is unusually deep for this account.
- *Source: Slack (#mixpanel-rubylabs), harvest-catchup 2026-07-02.*

**2026-06-10 | External channel scan [Jun 3 message — RESOLVED]**
- Customer asked about EU → US region transfer. Yonah replied: clarified it's about routing data to the correct endpoint, provided api_host config.
- Customer confirmed they solved it internally — keeping EU data residency, just needed to verify the correct api_host setup.
- Also Jun 2: platform-wide ingestion delay. Yonah + support replied, ticket #3511 closed, data intact.
