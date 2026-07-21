# MyHeritage Ltd. main account

**Last Updated:** 2026-07-14
**Updated By:** CSA OS (full harvest — Slack + BQ confirmation)
**Why Updated:** CONFIRMED — SFDC bot posted to #at-myheritage on 2026-07-13 13:27 IDT: Renewal opp moved `1: Pipeline` → `Closed Won` ("in 4 months"). This corroborates this morning's ambiguous Asana-digest signal with an actual Salesforce stage-change notification. However, the BQ `account_plans_current` table (refreshed as of this harvest) still shows `latest_opp_stage: 1: Pipeline` — BQ has not caught up to the SFDC change yet (expected; BQ sync lag). Treat the renewal as WON pending BQ catching up; the underlying EU data-residency and funnel-discrepancy issues remain open and unaffected by the renewal outcome.
**Previous:** 2026-07-06 — discrepancy open 55+ days, renewal Stage 1/High Risk (now believed stale).

---
## Sales Intelligence (BQ refresh — 2026-07-06, renewal status superseded 2026-07-13 per Slack/SFDC)
**Health:** Low (pre-renewal-close) | **Risk:** Was High, likely resolved — CONFIRM in SFDC directly | **Sentiment:** Neutral | **Opp:** MyHeritage FY27 Renewal (SFDC shows Closed Won as of 2026-07-13; BQ still shows Stage 1: Pipeline — sync lag)

**Active Risks:**
- EU data-residency deadline missed — MyStories project still sending ~1.26M events to the US endpoint.
- ~~Funnel/cohort percentage discrepancy open 55+ days without resolution.~~ RESOLVED 2026-07-16 per DM note.
- Renewal 25 days away, no formal motion advancing.
- Legal blockers preventing Session Replay adoption.

**Open Items:**
- [ ] Escalate and resolve EU endpoint issue for MyStories project — Yonah Dorfman — due 2026-07-10 (In Progress)
- [ ] Provide customer examples and legal reassurances for Session Replay — Shavit Ben-Itzhak — due 2026-07-10
- [x] Resolve 55+ day open funnel/cohort discrepancy with Itay Reznik — Yonah Dorfman — due 2026-07-12 (RESOLVED 2026-07-16 via DM note)
- [ ] Formally open and advance $110K renewal motion — Shavit Ben-Itzhak — due 2026-07-15 (In Progress)

Source: BQ (sales_intelligence.account_plans_current), morning-cycle harvest 2026-07-06.

---

## Identity

| Field | Value |
|-------|-------|
| ARR | $110,000 |
| Renewal | 2026-07-31 |
| Industry | Media & Entertainment |
| Segment | MM |
| Region | EMEA |
| Website | myheritage.com |
| AE | OferPolivoda (ofer.polivoda@mixpanel.com) |
| Sales Manager | Ghaith Ghazi |
| SFDC | [001i0000018aqPIAAY](https://mixpanel.lightning.force.com/lightning/r/Account/001i0000018aqPIAAY/view) |
| GDoc | [Account Plan](https://docs.google.com/document/d/18t4mYRonD5pAYGN5RIG24VJ8UqHSl8kvJfua9feYAOU/edit?usp=drivesdk) |

---

## Key Contacts

| Name | Title | Email | Interactions |
|------|-------|-------|-------------|
| yael.naor@myheritage.com | — | yael.naor@myheritage.com | 2 |
| itay.reznik@myheritage.com | — | itay.reznik@myheritage.com | 1 |

---

## Current Health

| Metric | Value |
|--------|-------|
| Overall | High |
| VHS Score | 9.2 |
| Adoption Score | 10.0 |
| Sentiment | Positive |
| Renewal Risk | Low |

High. The account is stable post-renewal with strong engagement and active learning from users.

---

## Executive Summary

As of 2026-05-31, MyHeritage has an active FY 27 Renewal in Pipeline for $110,000, closing 2026-08-01. There is also a Metric Trees upsell ($10K) in Stage 2: Solution. Last interaction: 2026-05-31 via Slack Message. Total Calls: 1. Total Emails: 15. The team shows healthy usage with 18K+ queries across 29 users. Strategic focus is on their [Scribe AI](https://familytreewebinars.com/webinar/rootstech-2026-recap-myheritages-announcements/) launch and expansion into [Whole Genome Sequencing](https://www.youtube.com/watch?v=9_X_X_X_X).

---

## Active Risks

- **[ACTIVE]** [2026-06-03] Potential ownership change with Francisco Partners exploring a sale
- **[ACTIVE]** [2026-06-03] Low adoption of premium features like Copilot and Replays


---

## Open Items

- [ ] **[DUE 2026-06-30]** Advance Metric Trees upsell ($10K) from Stage 2: Solution (Owner: Ofer Polivoda) — in progress
- [ ] **[DUE 2026-06-15]** Schedule next touchpoint (Owner: Gabriel Bruck) — pending
- [ ] **[DUE 2026-06-30]** Review and clean up the event lexicon in Mixpanel (Owner: Room-C-OY) — pending
- [x] **[DUE 2026-06-11]** Confirm with Itay Reznik that the May 10 funnel discrepancy (with/without exclude showing inverted %) was resolved — Yonah replied May 10 suggesting a UI display lag/refresh, but no customer confirmation received (Owner: Yonah Dorfman) — **RESOLVED 2026-07-16** per DM note
- [ ] **[OVERDUE — deadline passed 2026-07-01]** EU data residency: *MyStories* project still sending ~1.26M events/month to Mixpanel's US endpoint. Flagged Jun 21 in `#mixpanel-myheritage`, asked who owns the integration — no reply. (Owner: Yonah Dorfman) — needs urgent follow-up


---

## Sales Intelligence

| Field | Value |
|-------|-------|
| Latest Opp | MyHeritage Ltd. main account - FY 27 Renewal |
| Stage | 1: Pipeline |
| Opp ARR | 110000.0 |

---

## Recent History

**2026-07-19 | DM note [via Slack DM]**
- MyHeritage has closed a new contract and will start August 1.

**2026-07-16 | DM note [via Slack DM]**
- Funnel discrepancy is closed. Resolves the 55+ day open item confirming with Itay Reznik that the May 10 funnel/cohort % discrepancy was fixed.

**2026-07-06 | DM note [via Slack DM]**
- MyHeritage moved to the EU endpoint; will take some time for all app users to upgrade. (Progress signal on the overdue EU data-residency open item — not yet confirmed fully resolved.)

**2026-07-02 | External channel scan — EU data residency deadline missed**
- Jun 21: Yonah flagged MyStories project (~1.26M events) still routing to US endpoint, asked who owns the Mixpanel integration to get it sorted before Jul 1. No reply visible in channel. Deadline has now passed unresolved.
- *Source: Slack (#mixpanel-myheritage), harvest-catchup 2026-07-02.*

**2026-06-10 | External channel scan [May 10 message — REPLIED, UNCONFIRMED]**
- Customer shared funnel with % discrepancy when excluding users with error event (results showed inverted vs expected).
- Yonah replied May 10: compared two versions (47.36% vs 54.55%), said it's expected behavior. Customer clarified they were seeing the opposite values. Yonah replied: "Sometimes the visualization will lag and it just needs a refresh."
- No customer confirmation of resolution in thread. May have been a display glitch, but no close-out. Follow up with Itay Reznik before the Jul 31 renewal window.

**2026-06-23 | DM note [via Slack DM]**
- MyHeritage call rescheduled (Yonah confirmed via DM)

**2026-06-23 | EoD review**
- Call held 15:00 IDT today (Shavit + MyHeritage team). Post-call notes not captured — Yonah OOO 15:45. Itay Sobol had declined the invite; customer-side attendees unknown. Confirm outcome with Shavit tomorrow.
- ⚠️ Funnel discrepancy (May 10, Itay Reznik) still unreplied — 44 days. Must close before Jul 31 renewal window.
- ⚠️ Open items overdue: funnel discrepancy confirm (Jun 11), schedule next touchpoint (Jun 15 — may be closed by today's call), lexicon cleanup (Jun 30, 7 days remaining).

---
## V/R/G Log (EoD 2026-07-06)
**2026-07-06 | VALUE | EU endpoint migration progressing**
DM note from Yonah (13:55 IDT): MyStories has moved to the EU endpoint, with app users upgrading over time. This is genuine progress on the EU data-residency risk that had been open since the missed Jul 1 cutover. Not yet fully closed — "upgrading over time" implies a rollout tail, so keep this open until 100% of MyStories traffic is confirmed off the US endpoint. The separate 55+ day funnel/cohort discrepancy with Itay Reznik remains unresolved and is now the longest-open item in the book — did not move today.
*Source: DM note 2026-07-06 (13:55 IDT).*
