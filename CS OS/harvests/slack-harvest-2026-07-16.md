## Slack Harvest — 2026-07-16 (Full Catch-Up)

**Scope:** Explicit full catch-up run (not a rotation tick) — all 46 accounts scanned across 6 parallel batches, no batch-size cap. Every channel/external-channel/group-DM source scanned; 1:1 `user` rows scanned only where >30 days stale (monthly cadence), otherwise skipped as already covered.

**Unreplied Mentions (HIGH PRIORITY):** None found — every direct @-mention of Yonah across all 46 accounts had an in-thread reply.

**Unreplied Questions (open, customer waiting):**
- **DuxGroup** — #ext-mixpanel-duxgroup: customer asked (Jul 2, **14 days ago**) whether any action is needed on their end for the Snowflake warehouse connector re: EU data-residency migration. Still unanswered.
- **StakeMate** — #ext-mixpanel-stakemate: Pavle Andrić asked (Jul 14) for a React Native heatmap ETA; Yonah said "will look into it," no follow-up sent yet.
- **GETT** — #at-gett: Ofer Polivoda's Jul 6 question ("did we book the AI session Guy asked for?") was never explicitly closed out in-channel, even though the session did happen Jul 9 — worth a one-line confirmation reply.
- **mbg-digital.com** — #ext-mixpanel-mbgdigital: SR-overage/sampling conversation (Jul 12–13) ended on Yonah's message awaiting the customer's next step — not urgent, just not yet closed.

---

### Signals

---

#### Elementor — 🔴 CRITICAL — #at-elementor
- **From:** NPS survey bot (Jul 12)
- **Signal:** Contact Shahar B rated Mixpanel **0/10** at a $117K account that is simultaneously mid-renewal (Stage 5: Signing, $107,200, target close Jul 31) and just went through a ~30% layoff. A draft outreach email was auto-generated but needs a real, personal follow-up.
- **Type:** risk, sentiment, renewal
- **Reply status:** unreplied_mention-adjacent (needs proactive human outreach, not just the bot draft)

---

#### Stillfront Group AB (Playa Games) — 🔴 CRITICAL — #at-stillfront
- **From:** Gong call summary (Jul 9)
- **Signal:** Explicit churn risk flagged on the call — Playa Games studio may not renew ($12,348, Stage 1) due to underperformance/cost-cutting, despite Suman being positive on new AI features. Revisit planned for Q4. Separately, Nils Lobe's Apr 6 DM has reportedly sat unanswered 3+ months per internal briefing, though the group DM thread itself shows no messages since Jan 8 — his 1:1 DM was cadence-skipped this run (last fetched Jul 2) and should be manually checked next cycle rather than assumed fine.
- **Type:** risk, renewal, escalation
- **Reply status:** active_thread / needs manual check on Nils Lobe DM

---

#### Tabnine (Previously Codota) — 🟡 HIGH (de-escalating) — #at-tabnine
- **From:** Gong call summary (Jul 8)
- **Signal:** FY28 renewal ($93,073) — new proposal presented (~30M events/mo, $3,000/mo, 2-yr term). Ory Yassur open but hesitant on the 2-yr term, asked for a written proposal for legal review. This is a meaningful de-escalation from the prior active-cancellation-threat status (flagged critical in the Jun 9 harvest) — query volume is also rebounding. Written proposal still needs to go out.
- **Type:** renewal, decision
- **Reply status:** active_thread — action owed: send written proposal

---

#### ClickSoftware — 🔴 CRITICAL — #at-clicksoftware
- **From:** internal weekly digest / channel activity
- **Signal:** Zero named-user Mixpanel activity for 13+ consecutive weeks since renewal closed (Apr 15). Sole known contact Vince Valentino remains completely unresponsive. No Slack-side conversation exists to work with — this needs outreach through another channel.
- **Type:** risk, churn
- **Reply status:** noise (no customer engagement at all)

---

#### AT&T Israel — 🔴 CRITICAL — #at-and-t-global
- **From:** internal weekly digest
- **Signal:** Renewal ($63,205) lapsed since Jun 30 (16+ days), champion Ayelet Eyal unresponsive since Jun 15, SecurityScorecard vendor review still blocking. No new Slack activity — carried risk, not new this week.
- **Type:** risk, renewal
- **Reply status:** unreplied (Ayelet Eyal, off-Slack)

---

#### Base44 — 🟡 HIGH — #at-base44
- **From:** Gong call summary (Jul 15)
- **Signal:** FY27 renewal ($237,654, Stage 1) — 20-min call covered new agentic use cases (AI agent, Cross Analysis, custom properties via API, Mixpanel Headless). The QBR had been rescheduled twice and was confirmed for Jul 15 15:00–15:30 IDT — confirm it actually happened and capture outcomes.
- **Type:** renewal, milestone
- **Reply status:** active_thread — confirm QBR outcome

---

#### Bookaway — 🟡 HIGH — #at-bookaway
- **From:** Gong call summary (Jul 8)
- **Signal:** "Travelier" consolidation continues (Bookaway + One2Go Asia + a smaller EU brand unifying under one org), benchmarking Mixpanel vs. GrowthBook/internal tools — this is an AI-displacement-flavored risk. Renewal ($51,830) still Stage 1, no commercial motion yet; new Singapore-based GM budget owner not yet looped in.
- **Type:** risk, renewal
- **Reply status:** active_thread

---

#### BlueThrone — 🟡 HIGH — #ext-mixpanel-bluethrone / #at-bluethrone
- **From:** channel thread + internal digest
- **Signal:** A Lexicon Schemas/MCP support thread (Jun 29–Jul 3) resolved cleanly. Separately, an "Implementation Issue" alert fired Jul 10 on the Hoop project (cc'ing several BlueThrone contacts + Ofer Polivoda) that hasn't been independently confirmed resolved this run. FY28 renewal ($215,001) still Stage 1; the WHC enterprise trial window closed Jul 12 with no confirmed outcome.
- **Type:** risk, renewal
- **Reply status:** needs check-in on the Jul 10 implementation alert

---

#### Kape Technologies PLC (Israel) — 🟡 HIGH — #at-kape
- **From:** 4 Gong calls (Jul 8/9/12/14) + digest
- **Signal:** FY29 renewal ($115,000, Stage 1) mid relationship-reset — new AM (Shavit) introduced, decision-maker mapping and feature walkthroughs underway. Separately, an Implementation Issue alert fired Jul 10 on the ExpressVPN Apps project to a wide distribution list and still needs investigation.
- **Type:** renewal, risk
- **Reply status:** active_thread

---

#### Lemonade — 🔴 CRITICAL — #at-lemonade / DM
- **From:** internal digest + support ticket #9345
- **Signal:** Renewal ($80,284) is 5+ weeks overdue, still Stage 1 in SFDC despite an internal note suggesting it "has renewed" — needs confirmation with Tal Huberman. Separately, support ticket #9345 ("View Users not working" in Funnels) has been open since Jul 7 and unresolved after several rounds; Yonah's own Jul 12 DM follow-up to Leora is unanswered.
- **Type:** risk, renewal, support
- **Reply status:** unreplied (Leora, since Jul 12)

---

#### Loora.AI — 🟡 HIGH — #at-loora
- **From:** Gong call (Jul 16) + digest
- **Signal:** All May 19 QBR action items (GDPR guidance, funnel cutoff investigation, $15K pricing, mobile app implementation) are now 7+ weeks overdue; $20K FY27 Data Upsell stalled at Stage 2. Today's call requested a summary pricing table by end of July — MTU growth story is positive, so this is more "follow-through debt" than churn risk.
- **Type:** risk, expansion
- **Reply status:** action owed — pricing table by end of July

---

#### Movement-Group — 🟡 HIGH — #at-movement-group
- **From:** book-wide hygiene digest
- **Signal:** One of 10 accounts flagged for proactive outreach — single named user, near-zero feature adoption, no meetings scheduled. External channel is effectively abandoned (last real activity Jul 2025).
- **Type:** risk, churn
- **Reply status:** noise (no customer-side activity to react to)

---

#### Nexus Mods — 🟡 HIGH — #ext-mixpanel-nexusmods
- **From:** channel activity + digest
- **Signal:** Good news: new Data Analyst Nat Jones was introduced and a welcome call is booked — positive onboarding signal for a fresh champion. Less good: two support tickets (#9242, #10174) have breached SLA per the weekly digest and should get a status check. FY28 renewal $154,623 still Stage 1.
- **Type:** milestone, risk
- **Reply status:** resolved (intro) / needs check-in (SLA breaches)

---

#### PeerPlay — 🟡 HIGH — #at-peerplay
- **From:** Jul 6 QBR + digest
- **Signal:** QBR flagged possible "decoupling/churn risk," not yet formally confirmed. FY27 renewal ($90,999) is in Stage 4: Negotiation, and support ticket #5957 (Data Delay on Staging) status is unclear. No new Slack signal this week, but the churn-risk flag plus late-stage renewal makes this worth a direct follow-up.
- **Type:** risk, renewal
- **Reply status:** unresolved

---

#### Riverside.fm — 🟡 HIGH — #at-riverside
- **From:** internal weekly summaries
- **Signal:** Third+ consecutive quiet week with zero CSA engagement despite very high usage (39K+ queries/week, 62+ named users) — a real engagement gap on a healthy-usage account. SR-not-activated alert has been unassigned since May 29 (6+ weeks).
- **Type:** risk (engagement gap, not usage)
- **Reply status:** noise — proactive outreach overdue

---

#### PayBox - Discount Bank — 🟡 HIGH — #at-paybox
- **From:** internal digest
- **Signal:** $256K downgrade negotiation stalled 5+ weeks (Stage 4); EU data-residency follow-up with Yamin Elmakis unanswered; 8 open Session Replay action items from the May 13 demo. #mixpanel-paybox-qa channel is dead (Mixpanel disconnected after all members left).
- **Type:** risk, blocker
- **Reply status:** unreplied (Yamin Elmakis)

---

#### Papaya Global {Formerly Azimo} — 🟡 HIGH — #at-papayaglobal
- **From:** internal digest
- **Signal:** Hagit still hasn't given written renewal consent, blocking Stage 2; invoice #72667 ($11,700) is overdue; SR+WHC upsell ($15K) stuck. Hagit's Slack ID isn't in the manifest and couldn't be resolved this run — worth sourcing manually since she's the actual blocker.
- **Type:** risk, renewal, approval
- **Reply status:** unresolved

---

#### Simply — 🟡 HIGH — #at-simply
- **From:** internal hygiene digest
- **Signal:** Flagged as "needs outreach" — no recent rep-side touchpoint logged; SR UI-controls walkthrough with Roman overdue since May 27; FY28 Early Renewal + Upsell ($190K) stalled at Stage 1. Two data-quality support threads (Snowflake column dedup, `local_time` timezone conversion) both resolved cleanly this week.
- **Type:** risk, renewal, expansion
- **Reply status:** unresolved (outreach owed)

---

#### mbg-digital.com — 🟡 HIGH — #ext-mixpanel-mbgdigital
- **From:** channel thread (Jul 10–13) + digest
- **Signal:** Hit Session Replay limits; 105.5% overage already flagged Jul 1, invoice #72691 ($14,054) and billing ticket #699242 still open. Yonah is actively engaged on the sampling conversation (thread ended on his message, awaiting customer). Positive counter-signal: customer wants to expand into A/B testing/Experiments (not yet using the built-in feature) — real expansion opening once the overage/billing issue is resolved.
- **Type:** risk (overage/billing), expansion
- **Reply status:** active_thread

---

#### shortical.com — 🔴 CRITICAL — #at-shortical
- **From:** internal digest
- **Signal:** Red/critical — event overage now 125.7%+ and accelerating ~4%/week, customer actively weighing disabling Mixpanel for web, MTU pricing proposal 4+ weeks overdue. No Slack action visible addressing this yet.
- **Type:** risk, churn
- **Reply status:** unreplied — needs urgent outreach

---

#### Talon Cyber Security — 🟡 HIGH — DM
- **From:** DM thread (Jul 12–13)
- **Signal:** Yonah checked in on a prior action item; customer replied "super busy times, not yet, hopefully early August" — a slipping commitment consistent with the account's ongoing usage decline / red-circle health status. Exec-to-exec meeting still unconfirmed (carried over from the post-Palo-Alto-acquisition risk flagged in the Jun 9 harvest).
- **Type:** risk, renewal
- **Reply status:** resolved (acknowledged) but renewal risk persists

---

#### GameStory Israel / SuperPlay — 🟡 HIGH — #at-superplay
- **From:** 3 Gong calls (Jul 6/8/13)
- **Signal:** Good news: GameStory Israel's own $50,400 renewal closed won Jul 1. The bigger SuperPlay FY27 renewal ($867,562) is in active Stage 4 Negotiation — MTU tier redesign, discount options, and an "All-In" package proposal discussed across three calls. No direct action item surfaced for Yonah; monitor.
- **Type:** renewal, milestone
- **Reply status:** active_thread

---

#### RubyLabs — 🟡 HIGH — #mixpanel-rubylabs / #at-rubylabs
- **From:** channel threads + Gong call (Jul 8)
- **Signal:** An ingestion-delay incident (Jul 11) and a UI dropdown bug (Jul 10) both resolved same/next day. RubyLabs is building an AI skill for personalized offers and raised content-quality concerns re: Claude on the FY28 renewal call ($189,650) — needs Mixpanel guidance. Also surfaced a real product gap: no cache-bypass option for MCP queries, worth flagging to product.
- **Type:** renewal, decision, product gap
- **Reply status:** active_thread — content-quality guidance owed

---

#### SimilarWeb — 🟡 HIGH — #at-similarweb
- **From:** digest + Gong call (Jul 8)
- **Signal:** FY27 renewal ($94,777, Stage 1) — AI Agent + RCA Agent demoed, stage hasn't moved. Overdue action items: send Bnaya MCP/Lexicon docs (since Jul 1), address Yuval's AI-governance concern, confirm EU residency status, advance the SR+Experiments upsell.
- **Type:** renewal, risk
- **Reply status:** unresolved (multiple overdue action items)

---

#### Solaredge — 🟡 NORMAL — #at-solaredge
- **From:** Gong call (Jul 8)
- **Signal:** FY27 renewal ($61,478, Stage 1) — process-oriented call only (CSV export of contacts, identifying primary contact); no strong buying signal yet.
- **Type:** renewal
- **Reply status:** active_thread, low momentum

---

#### Guard.io — 🟡 HIGH — #guardio-mixpanel
- **From:** internal digest
- **Signal:** Account has been running at ~97–101% of plan ceiling for multiple weeks; the overage/tier conversation with Shavit remains unresolved. Shavit did send an AI-features intro to the customer this week (outbound, no reply needed).
- **Type:** risk (overage)
- **Reply status:** unresolved

---

### Positive / expansion signals worth acting on
- **GETT** — AI agent session (led by Yonah) went well Jul 9 and has already produced a potential Session Replay upsell for next quarter or two, per Tal Huberman — worth formalizing in SFDC.
- **AutoDS IL** — NPS 8 submitted Jun 26 following a Snowflake integration fix — good moment to ask for a reference or explore expansion.
- **Whalo** — Healthy/green account, migrating its "Brain" integration to MCP Headless SDK to resolve rate-limit friction; strong Copilot/Alerts adoption (895 MCP queries). $130K FY28 renewal in Stage 1 with no motion started yet — worth kicking off given the account health.
- **boinkers.io** — Shavit is pitching a customer-story feature around Boinkers' heavy Headless usage; Nick Lin has been looped in. Positive advocacy angle, though there's a recurring pattern of UI bugs (lookup tables, range fill, sampling discrepancy, now drag-and-drop) worth tracking as a cluster.
- **mbg-digital.com** — Customer wants to expand into A/B testing/Experiments (currently doing manual event-based tracking) — real expansion opening, best raised once the SR overage/billing issue is resolved.

---

### Quiet / low-signal this cycle (no action needed)
Altshuler Shaham, ClickSoftware (see critical note above — quiet doesn't mean fine here), Duda, Ferryhopper, Fibi - Israeli International Bank, Forex Club International LLC, Gamdom (one stale thread, see below), HAAT Delivery (resolved threads only), HoneyBook (active but on-track support threads), Nanit, Pango Pay & Go, Runwayer.com, StakeMate (see open question above), Taboola, MyHeritage (routine renewal tracking, one unanswered personal check-in), Appcharge, Fibi.

**Gamdom** worth a specific mention: an instrumentation-blocker thread from Jun 30 stalled after the customer said "will get an update today" on Jul 8 and never followed up (8 days stale) — ties to the account's flagged "critically low usage."

---

### Manifest / bookkeeping
- All 46 account manifests had their scanned Slack rows' `Last Fetched` set to 2026-07-16, with `Status` (Active/Inactive) refreshed per the 30-day recency rule.
- Rotation cursor reset: `slack-harvest-rotation-cursor.md` now shows `last_full_cycle_completed: 2026-07-16`, index reset to 0 for the next normal (capped) tick.
- No new Slack sources were discovered via search this run; no channel IDs errored with `channel_not_found`.
- Unresolved contact IDs flagged for manual lookup (left as-is, not guessed): Duda "Avigayil Lewin" (dup row), Elementor "Tehila" (dup of Tehila Adika), Pango "yardeny@pango.co.il", Papaya Global "Hagit" (the actual renewal blocker — worth prioritizing), Taboola "Artem Stolov", Whalo "Yaniv" (dup), mbg-digital.com "martin.lond@noclip.ee".
