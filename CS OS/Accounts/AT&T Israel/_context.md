# AT&T Israel

**Last Updated:** 2026-07-06
**Updated By:** CSA OS (morning-cycle BQ refresh)
**Why Updated:** Renewal now lapsed 6 days past Jun 30 expiry. SecurityScorecard review still blocking. Ayelet Eyal still unresponsive.
**Previous:** 2026-06-22 — legal hold confirmed, renewal due June 30.

---
## Sales Intelligence (BQ refresh — 2026-07-06)
**Health:** Low | **Risk:** High | **Sentiment:** Neutral | **Opp:** FY27 Renewal AT&T Israel & Metric Trees (Stage 4: Negotiation, $63,205)

**Active Risks:**
- Renewal expired Jun 30, now lapsed 6 days.
- Primary contact Ayelet Eyal unresponsive.
- SecurityScorecard review blocking renewal.

**Open Items:**
- [ ] Escalate renewal resolution to Ayelet Eyal and Harriet Kaufman — Tal Huberman — due 2026-07-06 (today)
- [ ] Route SecurityScorecard findings to Mixpanel security team — Tal Huberman — due 2026-07-07
- [ ] Propose short-term extension or bridge agreement — Tal Huberman — due 2026-07-08

Source: BQ (sales_intelligence.account_plans_current), morning-cycle harvest 2026-07-06.
**Previous:** Jun 15 — Ayelet session on attribution/source app tracking. Renewal 14 days out, no legal mention.

---

## Identity

| Field | Value |
|-------|-------|
| ARR | $59,995 |
| Renewal | 2026-06-30 |
| Industry | B2B |
| Segment | ENT |
| Region | EMEA |
| Website | www.corp.att.com/worldwide/att-you-israel |
| AE | TalHuberman (tal.huberman@mixpanel.com) |
| Sales Manager | Ofer Polivoda |
| SFDC | [0013q00002EvzATAAZ](https://mixpanel.lightning.force.com/lightning/r/Account/0013q00002EvzATAAZ/view) |
| GDoc | [Account Plan](https://docs.google.com/document/d/1IJuqHIUYJ_tO4Sf0H-zp76JgrkIqwpk-pAeqwcVrwdM/edit?usp=drivesdk) |

---

## Key Contacts

| Name | Title | Email | Interactions |
|------|-------|-------|-------------|
| Tal Huberman | — | Tal Huberman | 12 |
| ayelet.eyal@intl.att.com | — | ayelet.eyal@intl.att.com | 3 |
| chaya.benyehuda.dalven@intl.att.com | — | chaya.benyehuda.dalven@intl.att.com | 1 |
| ae3538@intl.att.com | — | ae3538@intl.att.com | 1 |

---

## Current Health

| Metric | Value |
|--------|-------|
| Overall | Medium |
| VHS Score | 6.7 |
| Adoption Score | 10.0 |
| Sentiment | Neutral |
| Renewal Risk | Medium |

Medium. Product usage is strong and the customer is actively engaging with support to refine their implementation. However, the pending renewal is still in negotiation and technical friction around user identification needs to be fully resolved.

---

## Executive Summary

There is an active pipeline deal for the FY27 Renewal ($63,205 ARR) currently in Stage 4: Negotiation. Last interaction: 2026-05-31 via Slack Message. Total Calls: 0. Total Emails: 30. The account shows healthy query volume (23K+ queries across 55 users), but there are ongoing technical hurdles regarding user identification (distinct_id vs attuid) that are being actively worked on via support. The primary focus is closing the renewal and ensuring the team can properly attribute events to users.

---

## Active Risks

- **[ACTIVE]** [2026-06-22] Legal hold blocking renewal — Yonah confirmed 11:04 IDT: "at&t is sitting with legal so there is nothing we can do." Jun 30 renewal ($63,205, Stage 4: Negotiation) now 8 days out. Passive posture — no commercial lever while in legal. Watch: if legal clears before Jun 30, immediate close push. If slips past Jun 30, escalate to Tal + Ofer for extension. Source: Slack DM note.
- **[ACTIVE]** [2026-06-03] Implementation issues regarding distinct_id and user attribution via the Node.js SDK.
- **[ACTIVE]** [2026-06-03] Obfuscated user IDs limit the ability to map specific champions and track individual adoption.
- **[ACTIVE]** [2026-06-15] Ayelet met with Yonah Jun 15 (live session) — walked through attribution / source app tracking. Issue resolved. Renewal lever established.


---

## Open Items

- [ ] **[DUE 2026-06-05]** Follow up with Ayelet Eyal on the missing event issue to confirm resolution. (Owner: Yonah Dorfman) — OVERDUE
- [ ] **[DUE 2026-06-15]** Advance FY27 renewal ($63K) from Stage 4: Negotiation to Closed Won. (Owner: Tal Huberman) — in progress
- [ ] **[DUE 2026-06-10]** Schedule a touchpoint to establish a structured meeting cadence. (Owner: Tal Huberman) — OVERDUE
- [x] **[DONE 2026-06-11]** Reply to Ayelet Eyal re: applicationFilterValue — replied Jun 10 + Jun 11 (computed properties / source breakdown). Thread ongoing — awaiting her response.


---

## Sales Intelligence

| Field | Value |
|-------|-------|
| Latest Opp | FY27- Renewal AT&T Israel & Metric Trees |
| Stage | 4: Negotiation |
| Opp ARR | 63205.0 |

### Mixpanel Telemetry
- **Billing ID:** 3061953
- **Plan usage:** 6.6% (event-based, renewal June 30 — 8 days out)
- **VHS:** 5.9 | **Adoption:** 10.0 | **Utilization:** 0
- **Note:** Utilization score = 0 is notable — despite high adoption score, features are not being deeply utilized. Legal hold is the immediate blocker; commercial conversation on hold.
- **Last refreshed:** 2026-06-22

---

## Recent History

**2026-07-06 | DM note [via Slack DM]**
- Contact clarification: Harriet Kaufman is an internal Mixpanel team member (not customer-side). Ayelet Eyal is a product user, not a decision maker on the renewal.

**2026-07-01 | Internal note [via Slack #at-and-t-global]**
- Harriet Kaufman (Mixpanel) added to the internal Slack channel (14:26 IDT). No customer-side signal on legal-hold status; renewal (Jun 30) remains unresolved. Confirm with Harriet/Tal why she was added — possible reinforcement on the stalled renewal.

**2026-06-15 | DM note [via Slack DM]**
- Met with Ayelet — walked her through attribution to find which apps users are coming from. She will work on it internally to figure out how they want to track it and the best approach, and will get back to Yonah.

**2026-06-11 | Gmail thread (latest)**
- Ayelet asked: wants to count unique source apps users arrived from; says source app property is unreliable
- Yonah replied Jun 11: suggested computed properties / source breakdown in Mixpanel
- Thread open — awaiting Ayelet's response
- Use thread resolution to advance renewal close (Stage 4, due Jun 30)

**2026-06-10 | Gmail signal**
- Ayelet Eyal emailed re: `applicationFilterValue` — Yonah replied Jun 10 asking for clarification

---
## V/R/G Log (EoD 2026-07-06)
**2026-07-06 | RISK | AT&T Israel — escalation plan needs correction**
Renewal now 7 days lapsed (expired Jun 30). Today's open item called for escalating to "Ayelet Eyal + Harriet Kaufman," but a DM note from Yonah today (13:55) clarifies: Harriet Kaufman is internal Mixpanel staff, not a customer-side escalation target, and Ayelet Eyal is a user, not a decision maker. This means the planned escalation path was miscast — need to identify the actual AT&T decision maker before any further outreach. No confirmation today that Tal's escalation happened, or to whom. Top priority for tomorrow: re-scope the escalation with the correct contact.
*Source: DM note 2026-07-06 (13:55 IDT) + BQ Sales Intelligence 2026-07-06.*
