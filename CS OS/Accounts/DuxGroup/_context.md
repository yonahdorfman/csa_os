# DuxGroup

**Last Updated:** 2026-06-22
**Updated By:** CSA OS (harvest-catchup — BQ/Mixpanel)
**Why Updated:** BQ telemetry: ⚠️ 97.9% plan usage on just-renewed account (Closed Won Jun 20). Expansion conversation needed before overage hits.

---

## Identity

| Field | Value |
|-------|-------|
| ARR | $78,810 |
| Renewal | CLOSED WON 2026-06-20 |
| Industry | Other |
| Segment | SMB |
| Region | EMEA |
| Website | https://duxgroup.com |
| AE | ShavitBen-Itzhak (shavit.ben-itzhak@mixpanel.com) |
| Sales Manager | Ofer Polivoda |
| SFDC | [001Nt00000C12C9IAJ](https://mixpanel.lightning.force.com/lightning/r/Account/001Nt00000C12C9IAJ/view) |
| GDoc | [Account Plan](https://docs.google.com/document/d/1QGOQYSVKgbuplzopUV6Jge6fYWr-XwrBzfLRE0tnzzw/edit?usp=drivesdk) |

---

## Key Contacts

| Name | Title | Email | Interactions |
|------|-------|-------|-------------|
| vzeinalov@marfa-tech.com | — | vzeinalov@marfa-tech.com | 1 |
| kkorolyova@noname.team | — | kkorolyova@noname.team | 1 |
| mmiadzvedz@marfa-tech.com | — | mmiadzvedz@marfa-tech.com | 1 |

---

## Current Health

| Metric | Value |
|--------|-------|
| Overall | Medium |
| VHS Score | 10.0 |
| Adoption Score | 10.0 |
| Sentiment | Mixed |
| Renewal Risk | Medium |

Health is moderate. While there is active negotiation for an upsell and renewal, the Session Replay momentum has stalled post-demo, and an open support ticket remains unresolved.

---

## Executive Summary

As of 2026-05-31, DuxGroup is in active negotiation for a FY 27 Renewal + Upsell + SR deal worth $79,000 ARR. Last interaction: 2026-05-31 via Slack Message. Total Calls: 1. Total Emails: 0. The account has moderate usage, but a recent Session Replay upsell demo has stalled and needs follow-up. DuxGroup is facing reputational risks due to an affiliate payment crisis reported in early 2026 ([GPWA](https://www.gpwa.org/forum/chilli-partners-has-closed-unconfirmed-outstanding-earnings-not-being-paid-263456.html)).

---

## Active Risks

- **[ACTIVE]** [2026-06-03] Session Replay upsell conversation has stalled since the April 14 demo.
- **[ACTIVE]** [2026-06-03] Open support ticket #695833 regarding custom roles is unresolved.
- **[ACTIVE]** [2026-06-03] Reputational and legal risks surrounding the company's ownership and affiliate payments.


---

## Open Items

- [ ] **[DUE 2026-06-05]** Follow up on Session Replay upsell post-demo. (Owner: Shavit Ben-Itzhak) — pending


---

## Sales Intelligence

| Field | Value |
|-------|-------|
| Latest Opp | DuxGroup - FY 27 Renewal + Upsell + SR - MEDIL |
| Stage | 4: Negotiation |
| Opp ARR | 79000.0 |

### Mixpanel Telemetry
- **Billing ID:** 4832267
- **Plan usage:** ⚠️ 97.9% — approaching limit (account Closed Won Jun 20 — renewal just completed)
- **VHS:** 10.0 | **Adoption:** 10.0
- **Expansion signal:** Near-limit usage on a fresh contract = strong upsell case for tier upgrade
- **Last refreshed:** 2026-06-22

---

## Recent History

**2026-07-21 | Slack full catch-up (#at-duxgroup, #ext-mixpanel-duxgroup, DM Vahshan Zeinalov, DM Ksenia Korolyova)**
- Weekly Slack account-summary bot (Jul 20) shows Session Replay activation still stalled — only 3 replays/week, unchanged for weeks, despite SR being sold as part of the June renewal ($78,810 closed won Jun 20).
- Query volume steady (~7,698/week); no new customer-initiated messages in #ext-mixpanel-duxgroup since the Jun 25–Jul 2 thread (Ksenia re-added to channel, Snowflake connector question resolved).
- Ksenia Korolyova DM and Vahshan Zeinalov DM show no new activity since Mar 30 — both quiet.
- No new signals beyond the recurring SR-activation gap already tracked.

**2026-06-10 | DM note [via Slack DM]**
- Waiting on their details; will be a $17K upsell — ball is in their hands now

---
## V/R/G Log (EoD 2026-07-06)
**2026-07-06 | RISK (flagged) → likely false positive | DuxGroup renewal date drift**
Today's janitor run flagged DuxGroup's Notion "Renewal Date" property as showing 2026-06-30, now 6 days lapsed, and not previously surfaced in any briefing. However, this account's own Context page already shows the FY27 renewal Closed Won on 2026-06-20 ($78,810). This looks like the same property-sync gap noted on PayBox today (child-page content updates not propagating to the parent DB rollup), not a real lapsed/at-risk renewal. Action: confirm with a /reconcile fix pass that the Notion database "Renewal Date" property is updated to reflect the Jun 20 close, so this doesn't keep re-triggering false alarms.
*Source: Janitor hygiene report 2026-07-06 + existing Context page state.*
