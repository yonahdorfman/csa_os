# Whalo

**Last Updated:** 2026-07-15
**Updated By:** CSA OS (morning cycle — Slack harvest)
**Why Updated:** API rate limit issue resolved; Brain integration migrating to MCP Headless.
**Previous:** 2026-06-21 — Gmail harvest catch-up (cohort sync limit increase).

---

## Machine Patch — 2026-07-15 (Morning Cycle)
### API rate limit issue resolved — migrating to MCP Headless
- Per Slack DM (Yonah/Shavit) and Linear ticket QE-441 ("Raise Mixpanel [API limit]...") marked Done 2026-07-08: Whalo's API rate limit has been increased, and their "Brain" internal AI tool (integrates with Mixpanel via API, was hitting a 60 req/sec limit) is migrating to MCP Headless (new cap 1000 req/min).
- This closes out the loop opened on the 2026-07-09 Gong call (Adir, Yaniv) where the rate-limit/Headless migration was first discussed.
- Remaining open item: confirm the Brain migration to MCP Headless completes smoothly and monitor new rate limit usage (Owner: Yonah).
- No renewal motion started yet on the $130,000 FY28 renewal (Stage 1: Pipeline) — worth opening given the account is otherwise healthy (45 active users, strong MCP/Copilot usage).
- Source: Slack #at-whalo + Linear QE-441, harvested via morning-cycle 2026-07-15.

---

## Identity

| Field | Value |
|-------|-------|
| ARR | $130,000 |
| Renewal | 2026-08-31 |
| Industry | Media & Entertainment |
| Segment | SMB |
| Region | EMEA |
| Website | whalo.com |
| AE | ShavitBen-Itzhak (shavit.ben-itzhak@mixpanel.com) |
| Sales Manager | Ofer Polivoda |
| SFDC | [0013q00002EAizuAAD](https://mixpanel.lightning.force.com/lightning/r/Account/0013q00002EAizuAAD/view) |
| GDoc | [Account Plan](https://docs.google.com/document/d/1j7Vp6mpsRAbMcgZA7lw21d3XRCc8fhXKSG_K9j9Evjk/edit?usp=drivesdk) |

---

## Key Contacts

| Name | Title | Email | Interactions |
|------|-------|-------|-------------|
| yaniv@whalo.com | — | yaniv@whalo.com | 2 |
| Shavit Ben-Itzhak | — | Shavit Ben-Itzhak | 1 |
| alon@whalo.com | — | alon@whalo.com | 1 |
| nitzan@whalo.com | — | nitzan@whalo.com | 1 |
| gabor.s@whalo.com | — | gabor.s@whalo.com | 1 |
| asaf@whalo.com | — | asaf@whalo.com | 1 |

---

## Current Health

| Metric | Value |
|--------|-------|
| Overall | High |
| VHS Score | 8.4 |
| Adoption Score | 10.0 |
| Sentiment | Neutral |
| Renewal Risk | Low |

High. The recent workshop on May 27 was successful, and platform engagement is strong across the team. The main gap is the lack of Session Replay adoption.

---

## Executive Summary

As of May 31, 2026, Whalo has an active pipeline deal 'Whalo - FY 28 Renewal' for $130,000 ARR. Last interaction: 2026-05-31 via Slack Message. Total Calls: 2, Total Emails: 8. A successful Mixpanel Workshop was recently completed at Whalo HQ on May 27, driving strong platform engagement across 51 active users. The primary focus is now on expanding adoption into Session Replay and Copilot, while ensuring the resolution of an active Zendesk ticket regarding property limits. Whalo is a high-growth mobile gaming studio focusing on 'social casino' games ([Startup Nation Central](https://startupnationcentral.org/company/whalo-games/)).

---

## Active Risks

- **[ACTIVE]** [2026-06-03] Lack of Session Replay adoption despite high query volume
- **[ACTIVE]** [2026-06-03] Pending support ticket regarding property limits


---

## Open Items

- [ ] **[DUE 2026-06-05]** Track resolution of ticket #701152 regarding Analytic Property Cut off. (Owner: Yonah Dorfman) — in progress
- [ ] **[DUE 2026-06-07]** Follow up post-workshop with Mor Lederhendler to gather feedback and define next steps. (Owner: Yonah Dorfman) — pending
- [ ] **[DUE 2026-06-15]** Pitch Session Replay to top human users eran.g, gabor.s, aviad. (Owner: Shavit Ben-Itzhak) — pending


---

## Recent History

**2026-06-29 | Gong/Clari — Mixpanel API Rate Limit call (16:05 IDT) [Yonah + Adir + Alon + Maayan]**
- Whalo's internal "Brain" tool hitting the legacy Query API's 60 req/hr limit as org-wide demand grows. Yonah pitched Mixpanel Headless (Python SDK) as the strategic fix; interim: raising rate limit to 600-1,000/hr. Standard MCP (600/hr) doesn't fit their server-side use case. New opens: send Headless docs (Yonah), raise rate limit (Yonah), follow up on Headless adoption (Yonah), investigate Adir's MCP connection issue (Adir). [Gong](https://us-16307.app.gong.io/call?id=2437408618794588326) / [Clari](https://copilot.clari.com/call/e6154603-a842-46a9-ac34-a99752046e1b)

**2026-06-10 | Quarterly meeting [via Slack DM note]**
- Quarterly meeting held today (Jun 10) with Alon and Yaniv
- Note: DM said "walo" — corrected to Whalo by Yonah

**2026-06-10 | DM note [via Slack DM]**
- Yonah confirmed: previous Slack DM reference to "walo" was intended as Whalo

**2026-06-18 | Support ticket resolved [Gmail]**
- Cohort sync limit increased to 120 for Whalo project 2305100 (Sergi Ferre, Mixpanel Support)
- Yonah had filed ticket Jun 10 requesting increase; now resolved — notify customer

---

## Sales Intelligence

| Field | Value |
|-------|-------|
| Latest Opp | Whalo - FY 28 Renewal |
| Stage | 1: Pipeline |
| Opp ARR | 130000.0 |

**2026-06-23 | DM note [via Slack DM]**
- Whalo was told (cohort sync / external slack confirmed) — check external Slack channel for confirmation

**2026-06-23 | EoD review**
- Cohort sync limit increase (to 120, Jun 18) confirmed communicated in external Slack channel. Pre-renewal quick win.
- ⚠️ Open item overdue: Track ticket #701152 — due Jun 5, 18 days overdue.
- ⚠️ Open item overdue: Post-workshop follow-up with Mor Lederhendler — due Jun 7, 16 days overdue.
- ⚠️ Open item overdue: Pitch SR to top users — due Jun 15, 8 days overdue.
- Renewal Aug 31 (69 days) — formal renewal conversation not yet started.

**2026-07-21 | Slack full catch-up (#at-whalo, #ext-whalo-mixpanel, DM Alon Lev, DM Yaniv Lior)**
- Customer confirmed a further rate-limit increase request (Fish of Fortune project, Jun 23): asked to bump from 60/hr to ~300/hr — Yonah replied same window increasing syncs 60→120, more may be needed.
- FY28 Renewal ($130,000, Stage 1: Pipeline) has its Aug 31 deadline ~6 weeks out with still no renewal motion started — flagged as the top open risk despite otherwise strong usage (43 active users, 285 MCP queries this week).
- Tracked feature usage (Value Moments, Replay, Copilot) dropped to zero this week vs. 821 Value Moments last week — worth a quick check whether this is a tracking gap or real change.
- DM channels with Alon Lev and Yaniv Lior (D0BAE63RHAL, D0B9MNGJA9F) returned no messages — likely wrong/stale IDs or no direct history; no signal to report there.
