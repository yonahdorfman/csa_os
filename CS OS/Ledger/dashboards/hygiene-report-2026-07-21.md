# Hygiene Report — 2026-07-21

**Run:** Janitor cycle (manually triggered ~11:20 GMT+3, backfilling missed 09:30am scheduled run)
**Book size:** 46 accounts

---

## Job 1: Context Freshness

✅ Fresh (≤7d): 11 accounts
⚠️ Stale (7-14d): 5 accounts — Fibi - Israeli International Bank, Forex Club International LLC, Solaredge, Taboola, boinkers.io BY Acid Labs
🔴 Critical (>14d): 30 accounts — AT&T Israel, Altshuler Shaham, Appcharge LTD, AutoDS IL, BlueThrone, Bookaway, ClickSoftware, Duda, DuxGroup, Elementor, Ferryhopper, Gamdom, GameStory Israel, Guard.io, HAAT Delivery, HoneyBook, Loora.AI, Movement-Group, Nanit, Pango Pay & Go ltd, Papaya Global {Formerly Azimo}, PeerPlay, Riverside.fm, SimilarWeb, Simply, StakeMate, Talon Cyber Security, mbg-digital.com, runwayer.com, shortical.com

**Read carefully:** most of these "critical" accounts have very recent real activity (see Job 8) — the local `_context.md` file just hasn't been re-written since a June patch even though Slack/BQ signals are current. Freshness here measures file-touch date, not relationship health.

---

## Job 2: Source Validation

**Validated this cycle: 42 accounts (27 priority + 15 rotation)** — see Validation Rotation below for methodology.

✅ All sources accessible: 41/42 accounts (Slack channel read + Notion page fetch spot-checks succeeded)

⚠️ **Broken source found:**
- **GameStory Israel** — Manifest lists Slack channel `C0130UR3BKK`, but that channel ID actually resolves to **#at-superplay**, containing SuperPlay account data ($840,000 renewal, Stage 4: Negotiation, MM segment) — not GameStory Israel. Either the manifest has the wrong channel ID, or the channel was renamed/repurposed and the manifest wasn't updated. **GameStory Israel has effectively had zero real Slack harvesting** for however long this has been wrong. Needs manual fix: find the correct GameStory Israel channel and update `_MANIFEST.md`.

Notion Job 5 spot-check (2 pages: Elementor, RubyLabs) — both pages fetched successfully.

---

## Job 3: Inbox

Pending: 4 items (none in `.processed/`)

- `2026-05-27-dm-notes.md` — Unmatched DM ("Check even import errors walo") — likely a truncated reference to **Whalo**. Recommend manual match/close.
- `2026-06-30-dm-notes.md` — Informational only (Gong setup confirmation, Clari→Gong transition). Safe to archive.
- `2026-07-16-unmatched.md` — 4 accounts (Island, Intel/Screenovate, Wilma Partners, Fireblocks) found in BQ with no local folder/Notion registry entry. Not yet triaged.
- `2026-07-19-dm-notes.md` — `/add-account` request for Wilma, Island, Fireblocks — duplicates the above, still pending action.

**Action needed:** Confirm whether Island/Intel/Wilma/Fireblocks are genuinely part of Yonah's book (CSA-name substring match could be an artifact) before onboarding via `/add-account`. This has now sat unactioned for 5+ days.

---

## Job 4: Structure + Sync

✅ 46/46 accounts have complete file sets (`_context.md` + `_MANIFEST.md`)
✅ 46/46 local account folders have a matching Notion registry entry (no orphans, no missing entries)

⚠️ **Field-naming inconsistency:** GETT's `_context.md` Identity table uses `Contract End` instead of the `Renewal` field every other account uses. This isn't just cosmetic — it means GETT's lapsed-renewal status doesn't get picked up by any automated scan that greps for `| Renewal |` (including this one, until manually corrected). **GETT's contract lapsed 2026-05-31 — 51 days ago — with the FY28 renewal opp still at Stage 1: Pipeline.** The account's own inline note says "LAPSED 37d," which is itself now stale/wrong (should read ~51d as of today). Recommend standardizing the Identity template field name and fixing the day-count math.

⚠️ **Notion header drift (2 of 2 spot-checked accounts affected):** Both Elementor and RubyLabs Notion Context pages show an inline "**Last Updated:**" summary line reading **2026-06-03** — even though both pages have newer dated patches appended below it (Elementor's Jun 30 DM-note patch; RubyLabs' Jul 15 Machine Patch). The append-only patch process isn't refreshing the top-of-page summary metadata, so anyone skimming just the header of a Notion page gets a falsely stale impression. Given this showed up in both of the 2 accounts spot-checked, it's likely book-wide — worth a dedicated audit/fix pass, not just a one-off.

Registry + drift check — this cycle's batch (42 accounts: 27 priority + 15 rotation): no additional page-not-found or registry-pointer issues found beyond the two items above.

---

## Job 5: Notion Health

✅ Accounts DB accessible
✅ 2/2 spot-checked pages OK (Elementor, RubyLabs) — see Job 4 drift note above
⚠️ Stale registry entries: none found

---

## Job 6: Renewals + Risk Alerts

🔴 **Renewal <60 days out:**
- **Elementor** — $117,200 — 2026-07-31 (10 days) — Stage 5: Signing, close call still unbooked, NPS 0 detractor unaddressed, MTU mismatch unresolved, 30% layoffs. **Highest-urgency item in the book.**
- **MyHeritage Ltd. main account** — $110,000 — 2026-07-31 (10 days) — ⚠️ per Slack, this actually **moved to Closed Won on Jul 13** and a new contract starts Aug 1. Local `_context.md` still shows Stage 1: Pipeline / Renewal Risk Low — needs a Notion/local update to reflect reality (good news, just needs to be recorded).
- **GameStory Israel** — $50,400 — 2026-08-01 (11 days) — Stage unclear; Slack channel validation was broken for this account (see Job 2), so recent state is unverified. Flag for immediate manual check given the renewal is 11 days out.
- **Whalo** — $130,000 — 2026-08-31 (41 days) — Stage 1: Pipeline, no renewal motion started yet despite the deadline approaching.

🔴 **Lapsed renewals, opp not Closed Won:**
- **GETT** — $66,602 — lapsed 2026-05-31 (**51 days overdue**, not 37d as the file states) — Stage 1: Pipeline. Lyft-acquisition contract authority still unresolved as the blocker.
- **Lemonade** — $67,000 — lapsed 2026-05-31 (51 days overdue) — Stage 1: Pipeline. Active $80K pipeline opp per Gong with a Jun 1, 2027 close date — worth checking whether this is genuinely the renewal vehicle or a separate motion.
- **AT&T Israel** — $59,995 — lapsed 2026-06-30 (21 days overdue) — Stage 4: Negotiation. Some forward motion: MSA signed (per Jul 19 Slack), SOW discussion Jul 20.

⚠️ **Critical health / High-or-Critical renewal risk (12 accounts):** Altshuler Shaham, ClickSoftware, Ferryhopper, Fibi - Israeli International Bank, Gamdom, Movement-Group, PayBox - Discount Bank, PeerPlay, Tabnine (Previously Codota), Taboola, Talon Cyber Security, mbg-digital.com

⚠️ **Overdue open items:** 44 of 46 accounts have at least one overdue open item. Worst: Altshuler Shaham (6), HoneyBook (6), Nexus Mods (5), boinkers.io (5), MyHeritage (4), PeerPlay (4), RubyLabs (4), Tabnine (4), shortical.com (4).

---

## Job 7: Communication Silence

🔴 **High Priority Outreach (46+ days silent): 2 accounts**
- **ClickSoftware** — 177 days silent (since 2026-01-25) — try: only known contact (Vince Valentino) has gone dark for 15+ weeks post-renewal; escalate outreach or formally flag as churn risk — this is the single most dormant relationship in the book.
- **Movement-Group** — 50 days silent (since 2026-06-01) — try: re-engage on the June VIP Event connection with Liat Levi (Marketing), which was never followed up on.

⚠️ **Needs Outreach (22-45 days silent): 8 accounts**
- **DuxGroup** — 41d silent (since 2026-06-10) — try: Session Replay was sold as part of the June renewal but usage is still only 3 replays/week — good excuse to reconnect on activation.
- **Ferryhopper** — 41d silent (since 2026-06-10) — try: team confirmed (via a Jul 19 outreach) they're in peak season; revisit the stalled nested-properties follow-up once that eases.
- **Loora.AI** — 41d silent (since 2026-06-10) — try: close out the ~9-week-overdue May 19 QBR action items (GDPR guidance, funnel cutoff, mobile app issue) — good, concrete reason to reach out.
- **Gamdom** — 36d silent (since 2026-06-15) — try: firm up the "early August" meeting that's been informally agreed but never scheduled.
- **runwayer.com** — 30d silent (since 2026-06-21) — try: Shavit's Jun 11 docs promise (A/B testing, feature flags, AI agent/MCP) is now 6+ weeks overdue to Valeria.
- **SimilarWeb** — 23d silent (since 2026-06-28) — try: follow up on the Lexicon-audit skill adoption question raised internally Jul 19-20.
- **Taboola** — 23d silent (since 2026-06-28) — try: reconcile the renewal figure/stage discrepancy ($189,650 Stage 4 per this week's internal briefing vs. $87,699 Stage 1 in the local file) — a legitimate, specific reason to get the account team aligned before reaching out.
- **Whalo** — 22d silent (since 2026-06-29) — try: kick off the $130K renewal motion given the Aug 31 deadline is now 6 weeks out.

**Exempt — Verify Manually:**
- **AT&T Israel** (WhatsApp) — 15d since last tracked contact — primary relationship runs through WhatsApp; confirm it's still active via that channel.

---

## Job 8: Unprocessed Activity Check (Possibly Stale Local Files)

🔄 **17 accounts** where real tracked activity is more recent than the local `_context.md` file reflects — these should be prioritized in the next harvest so nothing already-discussed goes stale in the record:

- **Appcharge LTD** — file 2026-06-03, activity as recent as 2026-07-21 (48d gap)
- **Simply** — file 2026-06-10, activity as recent as 2026-07-16 (36d gap)
- **Talon Cyber Security** — file 2026-06-03, activity as recent as 2026-07-07 (34d gap)
- **mbg-digital.com** — file 2026-06-03, activity as recent as 2026-07-06 (33d gap)
- **Duda** — file 2026-06-03, activity as recent as 2026-07-05 (32d gap)
- **BlueThrone** — file 2026-06-03, activity as recent as 2026-07-01 (28d gap)
- **Papaya Global {Formerly Azimo}** — file 2026-06-03, activity as recent as 2026-07-01 (28d gap)
- **Bookaway** — file 2026-06-10, activity as recent as 2026-07-06 (26d gap)
- **Guard.io** — file 2026-06-22, activity as recent as 2026-07-15 (23d gap)
- **Pango Pay & Go ltd** — file 2026-06-10, activity as recent as 2026-07-02 (22d gap)
- **Riverside.fm** — file 2026-06-21, activity as recent as 2026-07-07 (16d gap)
- **AutoDS IL** — file 2026-06-21, activity as recent as 2026-07-06 (15d gap)
- **HAAT Delivery** — file 2026-06-21, activity as recent as 2026-07-05 (14d gap)
- **Altshuler Shaham** — file 2026-06-21, activity as recent as 2026-07-02 (11d gap)
- **StakeMate** — file 2026-06-21, activity as recent as 2026-07-02 (11d gap)
- **SimilarWeb** — file 2026-06-21, activity as recent as 2026-06-28 (7d gap)
- **MyHeritage Ltd. main account** — file 2026-07-14, activity as recent as 2026-07-19 (5d gap) — **and this gap is the one that matters most: the local file predates the Jul 13 Closed Won update** (see Job 6).

**Biggest single find of this cycle:** MyHeritage's renewal actually closed (Closed Won, Jul 13) but the local file and the flagged-as-renewal-risk-Low record don't reflect that yet — good news sitting un-recorded rather than a risk, but worth fixing so future cycles don't waste attention re-flagging a closed deal.

---

## Validation Rotation (Jobs 2 & 4 methodology)

Cursor (`resources/janitor-validation-rotation-cursor.md`) was fresh/empty — no prior "last validated" dates existed. This cycle:
- **27 priority accounts** (auto-qualified: Renewal Risk High/Critical, lapsed contract not Closed Won, or ARR ≥ $100K) — validated in full, uncapped.
- **15-account rotation batch** (of 19 non-priority accounts, stalest-by-file-age-first): Duda, Papaya Global, mbg-digital.com, Bookaway, Pango Pay & Go, AutoDS IL, HAAT Delivery, Riverside.fm, SimilarWeb, StakeMate, runwayer.com, DuxGroup, Guard.io, HoneyBook, GameStory Israel.
- **4 accounts not yet validated this cycle** (carry to next rotation): Nexus Mods, Solaredge, Stillfront Group AB, shortical.com.

Cursor updated below.

---

## Summary

- **Warnings: 1** (rolled up per janitor.md format below)
- **Stale:** 5 | **Critical (file-age):** 30
- **Broken sources:** 1 (GameStory Israel — Slack channel mismatch)
- **Drift:** 2 accounts spot-checked, both showing Notion header-metadata staleness (likely book-wide pattern)
- **Renewals <60d:** 4 (Elementor, MyHeritage, GameStory Israel, Whalo)
- **Lapsed, not Closed Won:** 3 (GETT, Lemonade, AT&T Israel)
- **Overdue items:** 44/46 accounts have ≥1
- **Silence — high priority:** 2 (ClickSoftware, Movement-Group) | **needs outreach:** 8 | **exempt:** 1
- **Possibly stale (Job 8):** 17
