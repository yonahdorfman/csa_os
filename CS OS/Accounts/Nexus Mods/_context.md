# Nexus Mods

**Last Updated:** 2026-07-07
**Updated By:** CSA OS (Gmail catch-up — 14-day sweep, requested by Yonah)
**Why Updated:** New DA intro call scheduled with Natalie Jones (Jul 14) — closes the pending open item; new contact Britt.
**Previous:** 2026-06-15 — morning cycle refresh (tier upsell closed won).

---

## Identity

| Field | Value |
|-------|-------|
| ARR | $78,561 |
| Renewal | 2027-10-31 |
| Industry | Media & Entertainment |
| Segment | SMB |
| Region | EMEA |
| Website | www.nexusmods.com |
| AE | ShavitBen-Itzhak (shavit.ben-itzhak@mixpanel.com) |
| Sales Manager | Ofer Polivoda |
| SFDC | [0010Z000029U8qHQAS](https://mixpanel.lightning.force.com/lightning/r/Account/0010Z000029U8qHQAS/view) |
| GDoc | [Account Plan](https://docs.google.com/document/d/1a3bd1Lw-cDWDPpC_zvzyROhfSLLKHHFPuoOxWqtTWxM/edit?usp=drivesdk) |

---

## Key Contacts

| Name | Title | Email | Interactions |
|------|-------|-------|-------------|
| Shavit Ben-Itzhak | — | Shavit Ben-Itzhak | 8 |
| Yonah Dorfman | — | Yonah Dorfman | 2 |
| Dennis Raagaard | — | Dennis Raagaard | 1 |
| Natalie Jones (new DA, starts Jul 6) | — | natalie.jones@nexusmods.com | 1 (new, via Gmail 14-day sweep) |
| Britt | — | britt@nexusmods.com | 1 (new, via Gmail 14-day sweep — recurring Monthly Account Review) |

---

## Current Health

| Metric | Value |
|--------|-------|
| Overall | Medium |
| VHS Score | 10.0 |
| Adoption Score | 10.0 |
| Sentiment | Mixed |
| Renewal Risk | Medium |

Healthy but blocked by technical issues. Strong usage growth and expansion interest, but pending action items (Agreement, MCP EU setup) need immediate resolution.

---

## Executive Summary

As of 2026-05-31, Nexus Mods shows strong growth with an active pipeline deal 'Nexus Mods - FY 28 Renewal' for $101,466 ARR and an upsell opportunity in the Proof stage. Last interaction: 2026-05-31 via Slack Message. Total Calls: 1. Total Emails: 14. Usage is climbing (37K+ queries), and they are hiring a new data analyst to own Mixpanel. However, the 'Nexus Mods Agreement' task is overdue, and technical blockers (MCP EU setup, cookie consent) remain. The company recently saw a massive traffic milestone of 31M downloads in 24 hours ([Reddit](https://www.reddit.com/r/nexusmods/comments/18y5n5y/new_nexus_record_over_30_million_downloads_in_a/)).

---

## Active Risks

- **[ACTIVE]** [2026-06-03] Overdue Nexus Mods Agreement
- **[ACTIVE]** [2026-06-03] Unresolved MCP EU setup
- **[ACTIVE]** [2026-06-03] Cookie consent / pageview tracking issues
- **[ACTIVE]** [2026-06-10] Event volume overage — trending 2B/month (~24B/year) vs 15B/year contract. Pricing decision needed by Aug 1 renewal.
- **[RESOLVED 2026-07-02]** [2026-07-02] Unanswered GDPR/compliance question: customer's legacy desktop app can't be force-updated and will keep sending events to Mixpanel's US endpoint after the EU-forwarding deprecation (Jul 1). Customer explicitly asked (Jun 21) whether this traffic will simply be dropped after Jul 1. Resolved — answered in a different channel per Yonah (DM note, 18:58 IDT).



---

## Open Items

- [ ] **[DUE 2026-06-01]** Resolve overdue Nexus Mods Agreement (Owner: Yonah Dorfman) — OVERDUE
- [ ] **[DUE 2026-06-05]** Send Dennis pricing options for experiments (incl. 20% discount) (Owner: Shavit Ben-Itzhak) — OVERDUE
- [ ] **[DUE 2026-06-07]** Complete MCP EU setup documentation (Owner: Yonah Dorfman) — OVERDUE
- [ ] **[DUE 2026-06-12]** Send Dennis pricing email — 15B vs 24B tier comparison, Aug 1 renewal decision (Owner: Shavit Ben-Itzhak) — pending
- [x] **[DONE 2026-07-07]** Schedule intro call with new DA (starts July 6) — "Mixpanel + Nat - Welcome!" call confirmed with Natalie Jones for Jul 14, 2pm. (Owner: Yonah Dorfman)
- [ ] **[DUE 2026-06-22]** Send message to Nexus Mods — agent going live on Monday (Owner: Yonah Dorfman) [via DM]
- [x] **[DONE 2026-07-02]** ~~[URGENT — unanswered since 2026-06-21] Confirm to the customer whether legacy-desktop-app events hitting the US endpoint after Jul 1 will be dropped or still forwarded~~ — answered in a different channel per Yonah (DM note, 18:58 IDT). (Owner: Yonah Dorfman)


---

## Sales Intelligence

| Field | Value |
|-------|-------|
| Latest Opp | Nexus Mods - Tier Upsell |
| Stage | Closed Won |
| Opp ARR | 53157.0 |
| New ARR | 154622.0 (up from 101465) |

---

## Recent History

**2026-07-02 | DM note [via Slack DM] — RESOLVED**
- Yonah confirmed: "Nexus Mods questions were answered in a different channel, issue is closed."
- Closes the GDPR/compliance question below (legacy desktop app → US endpoint post-Jul 1 deprecation).

**2026-07-02 | External channel scan — unanswered GDPR/compliance question (flag for urgent action)**
- Jun 21: Yonah asked about status of API-forwarding migration from US to EU endpoint, target Jul 1.
- Jun 21 (later, 17:59): customer replied they can't complete the remaining work because it shipped in a desktop client with no forced-update path, and asked directly: "our understanding is that you'll be able to drop this traffic after July 1st?" — i.e. confirming Mixpanel will simply stop accepting/forwarding those events rather than exposing customer PII by routing to US infra.
- This question is still unanswered in the channel as of this scan. Earlier context (May 20) shows Marinus from Nexus Mods raised the same GDPR concern about an IDTA/compliance assessment being required if PII-bearing events get processed on US infrastructure — so this is a live, named compliance concern for the customer, not routine housekeeping.
- *Source: Slack (#ext-mixpanel-nexusmods), harvest-catchup 2026-07-02.*

**2026-06-17 | DM note [via Slack DM]**
- Agent going live on Monday (Jun 22) — reminder to send message to Nexus Mods then.

**2026-06-11 | Gmail (morning harvest) — Upsell deal confirmed**
- Dennis Raagaard replied Jun 10 to Shavit's email: "We would like to upgrade to the 24bn tier, we ofc want to wait for as long as possible before upgrading. It will be signed by Greg Cook."
- Shavit confirmed billing is annual, not monthly — no retroactive charges for prior months at lower tier.
- Greg Cook (Finance/Legal) to sign. Alexander Strands CC'd.
- Deal is essentially done — pending formal written consent + agreement.
- Action: Shavit to finalize. Yonah to ensure DA intro is scheduled for July 6 start.
- Source: Gmail thread "Mixpanel / Nexus Mods - Contract Updating" (Jun 8–10)

**2026-06-10 | Clari call signal [Jun 8 call]**
- Event volume trending ~2B/month (24B/year pace) vs 15B/year contract — overage risk
- Dennis needs pricing comparison email: 15B vs 24B tier options, before Aug 1 renewal decision
- New Data Analyst starting July 6 — schedule intro call; will own Mixpanel going forward
- $80K upsell pathway via tier upgrade (15B → 24B) — Shavit to lead
- Yonah to support pricing email and coordinate DA intro

**2026-06-10 | External channel scan [Jun 8 message — RESOLVED]**
- Customer asked why budget view showed 400M more events for May (200M more for April) vs All Events.
- Shavit replied same day: explained $all_mtu_events vs all events — MTU billing counts identify, merge, and other non-UI events. Customer: "Thx 🙌"
- Context: the discrepancy is expected behavior, not a bug. Ties into the pricing email — event volume discrepancy is the overage itself.
