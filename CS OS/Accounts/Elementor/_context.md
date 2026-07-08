# Elementor

**Last Updated:** 2026-06-30
**Updated By:** CSA OS (DM monitor)
**Why Updated:** Yonah confirmed conditional sampling checklist sent to Hagay. Open item resolved. Stage updated to 5: Signing (Jun 28).

---

## Identity

| Field | Value |
|-------|-------|
| ARR | $117,200 |
| Renewal | 2026-07-31 |
| Industry | B2B |
| Segment | MM |
| Region | EMEA |
| Website | www.elementor.com |
| AE | ShavitBen-Itzhak (shavit.ben-itzhak@mixpanel.com) |
| Sales Manager | Ofer Polivoda |
| SFDC | [0013q00002EttOWAAZ](https://mixpanel.lightning.force.com/lightning/r/Account/0013q00002EttOWAAZ/view) |
| GDoc | [Account Plan](https://docs.google.com/document/d/1ajp0MVzcFgBX71Ak4beflsJyVniZlf0UP2fkARIa0RA/edit?usp=drivesdk) |

---

## Key Contacts

| Name | Title | Email | Interactions |
|------|-------|-------|-------------|
| Shavit Ben-Itzhak | — | Shavit Ben-Itzhak | 3 |
| ofert@elementor.com | — | ofert@elementor.com | 1 |
| tehilaa@elementor.com | — | tehilaa@elementor.com | 1 |

---

## Current Health

| Metric | Value |
|--------|-------|
| Overall | Medium |
| VHS Score | 8.4 |
| Adoption Score | 10.0 |
| Sentiment | Mixed |
| Renewal Risk | Medium |

Medium. Usage is very strong and distributed across the team, but the MTU consumption gap (800K vs 1.5M) poses a commercial risk for the upcoming renewal.

---

## Executive Summary

As of May 31, 2026, Elementor has an active pipeline deal 'Elementor - FY 27 Renewal' for $117,200 ARR and a $35,000 Upsell in Proof stage. Last interaction: 2026-05-31 via Slack Message. Total Calls: 1, Total Emails: 30. A Mixpanel Team <> Elementor Intro is scheduled for June 4 to discuss the contract and A/B test tools. There is a known consumption mismatch (800K actual vs 1.5M projected MTUs) that needs addressing before the July renewal. Elementor operates a diversified Freemium SaaS model powering over 13% of WordPress sites ([Elementor](https://www.elementor.com/blog/website-builder-market-share-2026/)).

---

## Active Risks

- **[ACTIVE]** [2026-06-03] MTU consumption gap (actual 800K vs projected 1.5M)
- **[ACTIVE]** [2026-06-03] Experiments upsell is gated on Yotam's decision
- **[ACTIVE]** [2026-06-10] Tracking implementation issues — conditional sampling misconfiguration flagged on Jun 8 Sticklight call. Hagay needs a checklist from Yonah.
- **[ACTIVE]** [2026-06-10] $35K Upsell (Metric Tree + Experimentation + SR) Closed Lost Jun 9 — went from Stage 3: Proof to lost in 14 days. Understand why before Jul 31 renewal conversation.


---

## Open Items

- [x] **[DUE 2026-06-03]** Prep for Jun 4 Intro meeting to discuss contract and AB test tools. (Owner: Yonah Dorfman) — completed (meeting held Jun 4)
- [ ] **[DUE 2026-06-06]** Prepare Mixpanel Training session for Jun 7. (Owner: Shavit Ben-Itzhak) — status unknown
- [ ] ~~**[DUE 2026-06-15]** Advance $35K Experiments Upsell from Proof stage. (Owner: Shavit Ben-Itzhak)~~ — **CLOSED LOST Jun 9** — investigate loss reason before Jul 31 renewal
- [x] **[DONE 2026-06-30]** Send Hagay tracking issues checklist (conditional sampling fix) — from Jun 8 Sticklight call (Owner: Yonah Dorfman) — ✅ Confirmed sent Jun 30
- [ ] **[OVERDUE — deadline passed 2026-07-01]** EU data residency: ~21M events/month on `#mixpanel-elementor` project still routing to Mixpanel's US endpoint; flagged Jun 21 asking who owns the fix. No reply/owner confirmed in channel as of Jul 2. (Owner: Yonah Dorfman) — needs urgent follow-up


---

## Sales Intelligence

| Field | Value |
|-------|-------|
| Latest Opp | Elementor - FY 27 Renewal |
| Stage | 5: Signing |
| Opp ARR | 117200.0 |

---

## Recent History

**2026-06-30 | DM note [via Slack DM]**
- Yonah confirmed conditional sampling checklist sent to Hagay. Open item resolved (17d overdue, now done).
- Stage updated to 5: Signing per morning briefing (moved Jun 28).

**2026-06-15 | Slack harvest (morning cycle refresh)**
- Renewal advanced to Stage 4: Negotiation Jun 11 (from Stage 1: Pipeline) — Shavit driving close.
- Ghaith Ghazi (Mixpanel leadership) joined #at-elementor Jun 9 and asked "when do we expect the renewal to be signed?" — leadership urgency is real.
- Pylon Issue #5387 (super property attachment to events/profiles) approaching SLA breach.
- Sticklight call Jun 9: Hagay Lipman + Shavit — UTM persistence, proxy tracking, ad-blocker mitigation all open action items from Yonah.
- Source: Slack (#at-elementor)

**2026-06-10 | Clari call signals [Jun 4 + Jun 8 calls]**
- Jun 4: Intro meeting with Mixpanel team — discussed contract and AB test tools
- Jun 8 (Sticklight call): Tracking implementation issues flagged — conditional sampling misconfiguration. Hagay needs a checklist from Yonah.
- MTU consumption gap remains unresolved (800K actual vs 1.5M projected) — critical for Jul 31 renewal
- $35K Experiments upsell still gated on Yotam's decision — Shavit to push
- Renewal Jul 31 is 51 days away — tracking issues must be resolved before commercial conversation

**2026-07-02 | Group DM scan — Session Replay over-sampling investigated (technical, mostly closed)**
- Elementor engineer traced session-recording code (public GitHub repo) with Yonah, Jun 1: found sampling logic uses `distinct_id` hash (matches spec) but no persisted "false" decision / TTL for skipped users — replay was over-recording because sampling was reset on every qualifying event within a session rather than persisting per-tab. No fix committed in the thread; flagged as a gap the customer's team may still be working on.
- Separately, EU data residency reminder sent Jun 21 (see Open Items) remains open — deadline has now passed.
- *Source: Slack (group DM), harvest-catchup 2026-07-02.*

**2026-06-10 | External channel scan [Jun 9 signal]**
- $35K Upsell opp (Metric Tree + Experimentation + Session Replay) Closed Lost Jun 9 — went from Stage 3: Proof to lost in 14 days
- Lost before Yotam made a decision — investigate what happened (budget freeze? competitor win? internal reprioritization?)
- Understand loss reason before Jul 31 renewal conversation — risk of renewal churn if same objections apply
