# Base44

**Last Updated:** 2026-07-15
**Updated By:** CSA OS (morning cycle — Slack + Calendar harvest)
**Why Updated:** QBR today — Slack and Calendar disagree on the meeting time; flagged for verification before the call.
**Previous:** 2026-06-03 — BQ seed (initial).

---

## Machine Patch — 2026-07-15 (Morning Cycle)
### ⚠️ QBR today — scheduling conflict between sources, needs verification before the call
- Slack weekly summary (2026-07-12) says the Base44 QBR was rescheduled twice (Noa Gor declined the original Jul 8 slot) and is "now confirmed for Jul 15, 15:00–15:30 IDT," with confirmation status still flagged as unverified per Tal's internal pulse.
- **Calendar shows a conflicting time:** "Base44 // Mixpanel - Quarterly Account Meeting" on the primary calendar is set for 2026-07-15, 12:30–13:00 IDT (organizer Tal Huberman, attendee noagor@base44.com, Yonah RSVP still `needsAction`). There is also a separate "Base44" placeholder block 10:00-13:00 same day.
- **Action before the call:** confirm with Tal which time is correct (12:30 per calendar vs. 15:00 per Slack) and RSVP — do not show up assuming the Slack-reported time without checking, since the two sources disagree.
- Context for the call: strong week — MCP/agentic usage very high (2,696 API-query calls, 23 external users), Financial AI Hackathon co-hosted with RiseUp at Wix Campus, new product/swag update landed Jul 16.
- Source: Slack #at-base44 (weekly summary 2026-07-12) + Calendar (2026-07-15), harvested via morning-cycle 2026-07-15.

---

## Identity

| Field | Value |
|-------|-------|
| ARR | $122,764 |
| Renewal | 2026-11-30 |
| Industry | B2B |
| Segment | SMB |
| Region | EMEA |
| Website | base44.com |
| AE | TalHuberman (tal.huberman@mixpanel.com) |
| Sales Manager | Ofer Polivoda |
| SFDC | [001Nt00000URlJJIA1](https://mixpanel.lightning.force.com/lightning/r/Account/001Nt00000URlJJIA1/view) |
| GDoc | [Account Plan](https://docs.google.com/document/d/17QDlpuxL6pcR9WG6d6wZOV6LUT97g22N988toDVHR-w/edit?usp=drivesdk) |

---

## Key Contacts

_No contacts data available._

---

## Current Health

| Metric | Value |
|--------|-------|
| Overall | High |
| VHS Score | 10.0 |
| Adoption Score | 10.0 |
| Sentiment | Neutral |
| Renewal Risk | Low |

High health due to deep product adoption and cross-org usage from Wix. However, there is an open Data Pipeline issue that needs resolution, and the relationship needs to expand beyond the technical champion to senior leadership.

---

## Executive Summary

As of 2026-05-31, there is an active pipeline deal 'Base44 - FY 27 Renewal' for $122,800 ARR closing 2026-12-01. The account shows very strong engagement with 23K+ queries across 60 active users, including cross-org usage from parent company Wix. Last interaction: 2026-05-31 via Slack Message. Total Calls: 0. Total Emails: 30. Base44 recently reached $150 million ARR ([Seeking Alpha](https://seekingalpha.com/article/4695481-wix-after-layoffs-and-buyback-hold-for-now)), though parent company Wix announced a 20% workforce reduction ([Globes](https://en.globes.co.il/en/article-wix-plans-800-1000-layoffs-1001543841)).

---

## Active Risks

- **[ACTIVE]** [2026-06-03] Open Data Pipeline (DP) issue flagged for discussion.
- **[ACTIVE]** [2026-06-03] Parent company (Wix) undergoing massive 20% layoffs, which may disrupt integration or budget.
- **[ACTIVE]** [2026-06-03] Lack of usage in advanced features like Session Replay and Alerts despite high query volume.
- **[ACTIVE]** [2026-07-02] Unresolved data-integrity bug: customer reported duplicate events in the native Mixpanel→BigQuery export (May 12), asked for root cause and timeline. A dedup workaround was discussed May 14, but no confirmed root-cause/fix visible in channel.


---

## Open Items

- [ ] **[DUE 2026-06-01]** Discuss 'ticket - DP' in weekly sync and clarify action needed. (Owner: Tal Huberman) — pending
- [ ] **[DUE 2026-06-15]** Drive feature expansion (Session Replay and Alerts). (Owner: Tal Huberman) — pending
- [ ] **[DUE 2026-06-30]** Identify and engage a senior executive sponsor beyond the technical champion. (Owner: Ofer Polivoda) — pending
- [ ] **[OPEN since 2026-05-12]** Confirm root cause and resolution for duplicate events in the native BQ export bug. (Owner: Yonah Dorfman)


---

## Recent History

**2026-07-21 | Slack full catch-up [#at-base44, #ex-base44-mixpanel]**
- Base44 Quarterly Account Meeting held Jul 15 (after being rescheduled twice) — Noa Gor expressed satisfaction with current usage and strong interest in new capabilities: an AI Agent and Cross Analysis. Clear expansion signal.
- Customer asked for technical specs on integrating more deeply via Mixpanel's API (custom properties, "Mixpanel Headless"-style integration) — open task to send docs.
- MCP/agentic usage remains very strong, led by yoavfa@base44.com.
- No further update visible in #ex-base44-mixpanel on the May 12 duplicate-events/dedup BigQuery bug beyond what was already logged Jul 2 — still worth confirming root-cause closure.

**2026-07-02 | External channel scan — data integrity bug still open**
- May 12: customer reported duplicate events appearing in the native Mixpanel→BigQuery integration, asked for root cause/timeline. May 14: dedup workaround discussed. No confirmed close-out visible.
- *Source: Slack (#ex-base44-mixpanel), harvest-catchup 2026-07-02.*

## Sales Intelligence

| Field | Value |
|-------|-------|
| Latest Opp | Base44 - FY 27 Renewal |
| Stage | 1: Pipeline |
| Opp ARR | 122800.0 |
