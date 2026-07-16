# Lemonade

**Last Updated:** 2026-07-14
**Updated By:** CSA OS (full harvest — BQ + Slack correction)
**Why Updated:** Yonah confirmed ticket #9345 ("View Users Not Working") is resolved (DM note, 09:52). IMPORTANT CORRECTION found in this harvest: BQ `account_plans_current.commercials_contract_end` = **2027-06-01** — the CURRENT signed contract is not lapsed and doesn't expire for ~11 months. The "44 days past due" framing carried in prior briefings refers to the FY28 early-renewal opportunity's internal pipeline target (Stage 1, $80,284), not the actual contract end. #at-lemonade Slack (2026-07-12) shows an internal note was filed that Lemonade "has renewed," but SFDC still shows that opp at Stage 1 — this is a pipeline/forecast delay on the NEXT deal, not an active churn/lapse risk. Recommend correcting future briefings to stop describing this as a "lapsed renewal."
**Previous:** 2026-07-12 — morning-cycle Gmail harvest, weekly expansion.

---
## Sales Intelligence (BQ refresh — 2026-07-06, contract-date correction 2026-07-14)
**Health:** Low | **Risk:** Medium (downgraded from High — contract not actually lapsed, see correction above) | **Sentiment:** Negative | **Opp:** Lemonade FY28 Renewal (Stage 1: Pipeline, $80,284) | **Contract End:** 2027-06-01 (current contract active, not lapsed)
**Calendar note:** Lemonade / Mixpanel Quarterly Meeting today at 11:30 GMT+3.

**Active Risks:**
- Renewal 5+ weeks past due.
- Unresolved performance/slowness support ticket (#699316).
- Active `.set` tracking bug causing identity resolution issues.
- Three recent NPS detractors (alice.saugrain, kati.withnell, matan.rahav).

**Open Items:**
- [ ] Resolve renewal commercial discussion during QBR — Tal Huberman — due 2026-07-06 (today)
- [ ] Follow up on open Zendesk performance ticket #699316 — Yonah Dorfman — due 2026-07-07
- [ ] Address .set tracking bug / ID merge migration — Yonah Dorfman — due 2026-07-08
- [ ] Follow up with 3 NPS detractors — Tal Huberman — due 2026-07-09

Source: BQ (sales_intelligence.account_plans_current) + Calendar, morning-cycle harvest 2026-07-06.

---

## Identity

| Field | Value |
|-------|-------|
| ARR | $67,000 |
| Renewal | 2026-05-31 |
| Industry | Fintech |
| Segment | ENT |
| Region | EMEA |
| Website | lemonade.com |
| AE | TalHuberman (tal.huberman@mixpanel.com) |
| Sales Manager | Ofer Polivoda |
| SFDC | [0013100001gTudTAAS](https://mixpanel.lightning.force.com/lightning/r/Account/0013100001gTudTAAS/view) |
| GDoc | [Account Plan](https://docs.google.com/document/d/1xpXOWYMYLoza5ZosopH357TG08bA16lHSVa7FPckrGo/edit?usp=drivesdk) |

---

## Key Contacts

| Name | Title | Email | Interactions |
|------|-------|-------|-------------|
| Tal Huberman | — | Tal Huberman | 28 |
| Ran Shapira | — | Ran Shapira | 2 |
| leora.gildenblat@lemonade.com | — | leora.gildenblat@lemonade.com | 1 |
| dor.solomon@lemonade.com | — | dor.solomon@lemonade.com | 1 |
| ap@lemonade.com | — | ap@lemonade.com | 1 |

---

## Current Health

| Metric | Value |
|--------|-------|
| Overall | High |
| VHS Score | 6.7 |
| Adoption Score | 10.0 |
| Sentiment | Negative |
| Renewal Risk | Low |

Relationship is strong with a recently closed renewal, but there are active technical issues regarding user tracking (.set) and a recent NPS detractor.

---

## Executive Summary

As of 2026-05-31, Lemonade's FY 28 Renewal deal ($80,284 ARR) recently Closed Won. Last interaction: 2026-05-31 via Slack Message. Total Calls: 1. Total Emails: 30. Lemonade reduced its quota share cession rate to 20% to retain more premiums and launched Autonomous Car Insurance ([Insurance Business](https://www.insurancebusinessmag.com/us/news/reinsurance/insurtech-lemonade-scales-back-reinsurance-cession-506143.aspx), [Kavout](https://www.kavout.com/is-lemonades-ai-driven-model-finally-delivering-on-its-promise/)).

---

## Active Risks

- **[ACTIVE]** [2026-06-03] Mixpanel user tracking issue (.set)
- **[ACTIVE]** [2026-06-03] NPS Detractor (score 3)


---

## Open Items

- [ ] **[DUE 2026-06-04]** Follow up on Mixpanel user tracking issue (.set) (Owner: Yonah Dorfman) — pending
- [ ] **[DUE 2026-06-03]** Session Replay meeting with Leora Gildenblat (Owner: Tal Huberman) — pending
- [ ] **[OPEN since 2026-06-30]** Customer requested a Mixpanel onboarding session for new PMs and analysts who've joined the team — good adoption/expansion signal, needs scheduling. (Owner: Tal Huberman)
- [x] ✓ Ticket #9345 "Lemonad - View Users Not Working" — Support (Lin Yee) narrowed it to a missing `preventative_items_select_question_answered` event in the funnel (Jul 8); Yonah replied same day with the funnel link. Confirmed resolved per Yonah's 07-14 DM note. (Owner: Yonah Dorfman)


---

## Sales Intelligence

| Field | Value |
|-------|-------|
| Latest Opp | Lemonade - FY 28 Renewal |
| Stage | 1: Pipeline |
| Opp ARR | 80284.0 |

## Recent History

**2026-07-12 | Gmail harvest (morning-cycle, weekly) — ticket #9345 stalled on customer side**
- Support (Lin Yee) diagnosed the "View Users Not Working" issue Jul 8: the funnel Yonah shared didn't include the `preventative_items_select_question_answered` event, which is likely the root cause. Yonah replied same day (Jul 8, 07:29 IDT) with the funnel link and analysis.
- No reply from Leora Gildenblat or anyone at Lemonade since. Mixpanel Support auto-sent a "haven't heard from you" follow-up on Jul 11 (09:05 IDT).
- Separately, a general Mixpanel platform incident ("Temporary Data Ingestion Delay for US Projects") ran Jul 11 09:05-14:25 IDT and was resolved same day — worth a quick mention to Lemonade only if they ask, not proactively, since it's fully resolved and account-specific impact wasn't confirmed.
- Action: nudge Leora directly (not just via the ticket) to close out #9345 — it's been open 5 days with the fix path already identified.
- Source: Gmail, morning-cycle harvest 2026-07-12.

**2026-07-07 | Gmail catch-up (14-day sweep) — new support ticket + call summary forwarded**
- New support ticket #9345 "View Users Not Working" submitted 07:10 IDT via support@mixpanel.com — no reply yet as of this scan.
- 07-06 11:37: Tal Huberman forwarded a Clari Copilot call summary for the "Lemonade / Mixpanel - Quarterly Meeting" (same QBR flagged in the 07-06 V/R/G log as "outcome not yet captured") — the forward itself doesn't contain the summary text in the snippet scanned, so the actual QBR outcome (renewal commercial discussion, .set bug, NPS detractors) still needs confirming directly with Tal.
- Source: Gmail, 14-day catch-up harvest 2026-07-07.

**2026-07-02 | External channel scan — new onboarding request (expansion signal)**
- Jun 30: customer asked if Mixpanel can run an onboarding session for new PMs and analysts joining the team — organic seat growth, worth scheduling promptly and flagging to the AM as a positive signal ahead of renewal conversations.
- *Source: Slack (#mixpanel-lemonade-shared), harvest-catchup 2026-07-02.*

**2026-07-14 | DM note [via Slack DM]**
- Yonah: "Lemonade ticket is resolved" — confirms ticket #9345 ("View Users Not Working") is closed. Open Item marked resolved. Note: a separate Zendesk performance ticket #699316 is also referenced in the Sales Intelligence block above — ambiguous whether Yonah meant that one instead; worth a quick confirmation if #699316 is still open.

**2026-07-08 | DM note [via Slack DM] — renewal correction**
- Yonah confirmed: "Lemonade has renewed" — flags that CS OS's lapsed/pipeline renewal status is out of date. SFDC/BQ Sales Intelligence still shows FY28 Renewal opp in "1: Pipeline" as of the 2026-07-06 refresh; needs SFDC reconciliation to confirm the correct stage/close date before next hygiene run stops flagging it as lapsed.

**2026-06-23 | EoD review**
- Slowness ticket (Leora Gildenblat, ~Jun 16) status unconfirmed — not checked today.
- ⚠️ Open item overdue: .set tracking follow-up — due Jun 4, now 19 days overdue.
- ⚠️ Open item overdue: Session Replay meeting with Leora — due Jun 3, now 20 days overdue.
- No account touch today. Post-renewal follow-up cadence has lapsed.

---
## V/R/G Log (EoD 2026-07-06)
**2026-07-06 | RISK | Quarterly Meeting held — outcome not yet captured**
11:30 QBR occurred per calendar. Goal going in was to force the overdue renewal commercial conversation (FY28, $80,284 pipeline) without letting the .set tracking bug or NPS detractors dominate the call. No Gong/Clari record or Slack update available yet to confirm what was actually discussed or decided — same "call happened, outcome unknown" pattern seen on other accounts this cycle. Follow up with Tal Huberman tomorrow to confirm renewal status, whether the .set ticket (#699316) got attention, and whether the 3 NPS detractors were addressed.
*Source: Calendar 2026-07-06 + BQ Sales Intelligence 2026-07-06.*
