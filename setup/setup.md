---
name: Setup CSA OS v3
version: 3.3
last_updated: 2026-04-22
category: setup
description: >
  Creates a complete CSA OS workspace from scratch. Discovers accounts
  from BQ, verifies Slack channels, builds local folder tree, creates
  Notion Accounts DB with parent + 3 child pages per account.
  Defaults Slack dispatch to personal DM and enables DM monitor.
invoke:
  - "/setup-csa-os"
  - "set up the context daemon"
---

# Setup CSA OS v3

Single entry point. Creates everything from zero.

---

## Phase 0 — Probe Environment

### 0.1 Probe MCPs

Test each MCP. Record what's available.

| MCP | Required | Test Call |
|-----|----------|----------|
| Notion | **Yes** | `notion-search` for any page |
| Slack | **Yes** | `slack_search_users` for the CSM |
| BigQuery | Recommended | `SELECT 1` from BQ |
| Gmail | No | `gmail search_threads` |
| Google Calendar | No | `gcal list_calendars` |
| Google Drive | No | `drive search_files` |

If Notion or Slack missing → STOP.
If BQ missing → warn, accounts must be added manually.

### 0.2 Detect Existing Workspace

Check if `/CS OS/` or `/Cowork/` exists.
- Found with accounts → upgrade mode (preserve content)
- Found empty → partial install
- Not found → fresh install

---

## Phase 1 — Identity

Ask ONE question at a time:

1. "What's your full name?" (used to match BQ `csa` column)
2. "Company?" (default: Mixpanel)
3. "Title?" (default: Customer Solutions Architect)
4. "Timezone?" (detect from system, confirm)
5. "Work email?"
6. "Slack user ID?" (instructions: click avatar → Profile → ⋮ → Copy member ID)

Store answers. These fill placeholders in all subsequent steps.

---

## Phase 2 — Account Discovery

### 2a — Try BigQuery First

**If BQ MCP connected:** Run the discovery query.

```sql
SELECT
  m.account_name, m.csa, m.slack_channel_id_c,
  m.arr AS metadata_arr, m.industry, m.segment, m.region,
  m.account_owner, m.account_owner_email, m.sales_manager,
  m.health_score_vhs, m.health_score_adoption,
  m.sfdc_account_id, m.website,
  p.arr AS plans_arr, p.commercials_contract_end,
  p.tldr_health_rating, p.risk_renewal_level,
  p.sentiment_label, p.executive_summary,
  p.contacts_json, p.action_plan_json,
  p.strategic_goals_json, p.risk_identified_risks_json,
  p.health_overall_assessment, p.gdoc_link,
  p.latest_opp_name, p.latest_opp_stage, p.latest_opp_bookings_arr
FROM `mixpanel-sa.sales_intelligence.account_metadata` m
LEFT JOIN `mixpanel-sa.sales_intelligence.account_plans_current` p
  ON m.account_name = p.account_name
WHERE LOWER(m.csa) LIKE '%{csm_name_lower}%'
ORDER BY COALESCE(p.arr, m.arr) DESC
```

**Key tables:**
- `account_metadata` has: `csa` (CSA name), `slack_channel_id_c` (Slack channel), `sfdc_account_id`
- `account_plans_current` has: rich context — executive summary, contacts, risks, action plans, health

**If BQ query succeeds:** Present the roster for confirmation (see below).

**If BQ MCP probe passed but query times out or errors:**
This happens — BQ connections can be flaky. Try once more with a simpler query:
```sql
SELECT account_name, arr, csa, slack_channel_id_c
FROM `mixpanel-sa.sales_intelligence.account_metadata`
WHERE LOWER(csa) LIKE '%{csm_name_lower}%'
ORDER BY arr DESC
```
If the simpler query works → use metadata-only (no rich context from account_plans).
If it also fails → fall back to manual entry.

### 2b — Manual Entry (BQ unavailable or as supplement)

**If BQ not connected, times out, or returns no results:**

Ask: "Let's add your accounts manually. You can also add more later with `/add-account`."

For each account, ask:
1. "Account name?"
2. "ARR?" (or skip)
3. "Renewal date?" (or skip)
4. "AE name?" (or skip)

Accept accounts one at a time. User says "done" when finished.

**The user can ALWAYS add accounts later with `/add-account`.** Setup doesn't
need to capture every account — the harvesters will discover others from
Gmail, Slack, and Calendar signals over the coming days.

### 2c — Present Roster for Confirmation

Whether from BQ or manual entry, present the combined list:

```
## Your Accounts ({N})

| # | Account | ARR | Renewal | Health | Source |
|---|---------|-----|---------|--------|--------|
| 1 | Workday | $541K | Jul 2026 | Medium | BQ |
| 2 | DocuSign | $537K | Jul 2026 | Medium | BQ |
| 3 | McDonald's | $200K | — | — | Manual |
...

Make edits — remove, add, or correct. Then confirm.
```

**WAIT for confirmation before proceeding.**

### 2d — Verify Slack Channels

For each account with a `slack_channel_id_c` from BQ:
1. Call `slack_read_channel` with `channel_id={id}`, `limit: 1`
2. Record the resolved channel name
3. If channel not found → note it, ask user for correct ID

For accounts without a channel (manual or BQ without channel):
1. Search Slack for `at-{account_safe_name}`
2. If found → suggest. If not → leave blank.
3. Note: channel can be added later by editing the account's `_MANIFEST.md`

---

## Phase 3 — Create Local Folder Tree

Create this structure under the workspace root (e.g., `/CS OS/`):

```
{ROOT}/
├── Accounts/
│   ├── _MANIFEST.md              (index table: account name, ARR, renewal, health, slack channel)
│   └── {Account Name}/           (one per confirmed account)
│       ├── _context.md           (seeded from BQ — see Phase 3b)

│       ├── _MANIFEST.md          (sources: Slack channel, BQ, SFDC, GDoc)
│       ├── call-notes/
│       ├── assets/
│       └── archive/
├── Ledger/
│   ├── changelog/
│   ├── briefings/
│   ├── retros/
│   ├── monthly/
│   └── dashboards/
├── Personal Context/
│   └── about-me.md               (filled from Phase 1)
├── Global Context/
│   └── README.md
├── Team Context/
│   └── README.md
├── Inbox/
│   └── .processed/
└── resources/
    ├── skills/global/             (install kit: sub/, roles/, cadence/)
    ├── skills/personal/
    │   └── overrides.md           (filled from Phase 1 + 2)
    ├── templates/
    ├── cadence.md
    └── notion-page-registry.md    (written in Phase 5)
```

### 3b — Seed _context.md from BQ

For each account with `account_plans_current` data, build `_context.md`:

**Header:**
```
# {account_name}

**Last Updated:** {today}
**Updated By:** CSA OS (BQ seed)
**Why Updated:** Initial seeding from BigQuery account_plans_current
```

**Identity section:** From `account_metadata` — name, ARR, renewal, AE, industry, website, SFDC link.

**Key Contacts:** Parse `contacts_json`. Filter entries where name = "None" or email = "None". Deduplicate by email. Sort by interaction_count desc.

**Current Health:** From `health_overall_assessment`. Include VHS, adoption scores, sentiment label, risk level.

**Executive Summary:** From `executive_summary` — keep it verbatim, it's already well-written.

**Active Risks:** Parse `risk_identified_risks_json`. Create dated entries with today's date and status [ACTIVE].

**Open Items:** Parse `action_plan_json`. Create checklist entries: `- [ ] **[DUE {timeline}]** {task} (Owner: {owner}) — {status}`. Mark completed items as `- [x]`.

**Recent History:** Extract interaction dates/summaries from `executive_summary`.

**Sales Intelligence:** VHS, adoption, sentiment, latest opportunity details.

For accounts WITHOUT `account_plans_current` (null plan data):
Seed from `account_metadata` only — Identity section + whatever scores are available.

### 3c — Seed _MANIFEST.md (per account)

```
# {account_name} — Manifest

## Sources

| Source | Type | Identifier | Status | Last Fetched |
|--------|------|-----------|--------|-------------|
| Slack | channel | #{channel_name} ({channel_id}) | Active | {today} |
| BigQuery | sales_intelligence | account_plans_current | Active | {today} |
| SFDC | account | [{sfdc_id}](https://mixpanel.lightning.force.com/lightning/r/Account/{sfdc_id}/view) | Linked | — |
| GDoc | account_plan | [Link]({gdoc_link}) | Active | — |

## Collaborators

| Name | Role | Email |
|------|------|-------|
| {CSM_NAME} | CSA | {CSM_EMAIL} |
| {account_owner} | AE | {account_owner_email} |
| {sales_manager} | Sales Manager | — |
```

---

## Phase 4 — Slack Configuration

### 4a — Dispatch Destination (default: personal DM)

1. **Fetch user's Slack profile** using `slack_read_user_profile` with the user ID from Phase 1.
   Confirm the display name matches. Store the verified user ID.

2. **Ask:** "Where should briefings, retros, and alerts post?"
   - **Personal DM** ← default, recommended
   - A shared channel (user provides channel ID)

3. **If Personal DM (default):**
   - Store in overrides and cadence.md:
     ```
     DISPATCH_TYPE=dm
     DISPATCH_TARGET={SLACK_USER_ID}
     ```
   - Post test message to user's DM: "🟢 CSA OS connected. Briefings will post here. Post `help` for commands."
   - Confirm: "✅ Dispatch set to your personal DM."

4. **If shared channel:**
   - Ask for channel ID
   - Verify with `slack_read_channel` (limit: 1)
   - Store:
     ```
     DISPATCH_TYPE=channel
     DISPATCH_TARGET={CHANNEL_ID}
     ```
   - Post test message to channel
   - Confirm: "✅ Dispatch set to #{channel_name}."

### 4b — DM Monitor (default: enabled, 30 min)

The DM monitor watches your personal Slack DM for notes and context updates.
When you post something like "Workday: spoke with Chad, all events fix confirmed for May"
the monitor picks it up, matches it to the account, and files it to context.

1. **Ask:** "Monitor your DMs for account notes every 30 minutes?"
   - **Yes, every 30 minutes** ← default, recommended
   - Yes, but different interval (ask for minutes)
   - No, I'll update context manually

2. **If Yes (default):**
   - Add to cadence.md:
     ```
     every 30m during work_hours → slack-dm-monitor
     ```
   - Store in overrides:
     ```
     DM_MONITOR_ENABLED=true
     DM_MONITOR_INTERVAL=30m
     ```
   - Create Cowork scheduled task: "Slack DM Monitor" at `*/30 * * * *`
   - Confirm: "✅ DM monitor active. Post account notes to your DM anytime."

3. **If custom interval:**
   - Same as above but with user-specified interval

4. **If No:**
   - Store `DM_MONITOR_ENABLED=false`
   - Skip task creation
   - Note: "You can enable this later by adding `every 30m → slack-dm-monitor` to cadence.md"

### 4c — Telemetry Channel

No prompt needed — this happens automatically.

1. Invite the user to `C0B1ZU8QSHE` (#csa-os-telemetry)
2. Confirm: "✅ Added you to #csa-os-telemetry (usage tracking — you can mute this channel)"
3. Store in overrides: `TELEMETRY_CHANNEL=C0B1ZU8QSHE`

If invite fails (channel doesn't exist or permissions issue), log warning and continue.
Telemetry is non-blocking — setup completes without it.

---

## Phase 5 — Notion Setup

### 5.1 Create or Locate Accounts Database

Ask: "Do you have an existing Accounts database in Notion, or should I create one?"

**If creating new:**
Create database with these properties:
- `Name` (title)
- `Health` (select: healthy, at_risk, critical)
- `Renewal Date` (date)
- `Last Updated` (date)

**If existing:**
Fetch it. Verify it has the 4 required properties. Store the database ID and data source ID.

### 5.2 Create Account Pages with Child Pages

**Structure per account:**

```
Accounts DB
└── {Account Name}           ← parent page (DB entry, properties only)
    ├── Context              ← child page (full account context — read every cycle)
    └── Manifest             ← child page (sources — read at setup/sync)
    └── Contacts             ← child page (contacts that should be updated — read at setup/sync)
```

**CRITICAL — Process ONE account at a time to prevent parent/child mapping bugs.**

The v3.1.0 seeding had a bug where child pages were nested under the wrong
parents because multiple parents were created in a batch and the IDs got
mixed up. The fix: create parent → record ID → create children → record IDs
→ THEN move to the next account. Never batch across accounts.

**For EACH account, in sequence:**

**Step A: Create parent page**
```
notion-create-pages:
  parent: { data_source_id: "{DATA_SOURCE_ID}" }
  pages: [{
    properties:
      Name: "{account_name}"
      Health: "{health}"
      date:Renewal Date:start: "{renewal_date}"
      date:Last Updated:start: "{today}"
  }]
```
Record returned `page_id` immediately. This is `{parent_id}`.

**Step B: Create 2 child pages under THAT SPECIFIC parent**
```
notion-create-pages:
  parent: { page_id: "{parent_id}" }    ← from Step A
  pages: [
    { properties: { title: "Context" }, content: "{_context.md}" },
    { properties: { title: "Manifest" }, content: "{_MANIFEST.md}" }
  ]
```
Record the 2 returned child page IDs.

**Step C: Write to registry immediately**
```
ACCOUNT:{name}={parent_id}
ACCOUNT:{name}:context={context_id}
ACCOUNT:{name}:manifest={manifest_id}
```

**Step D: Verify**
Fetch the parent page. Confirm it shows 2 child page links (Context, Manifest).
If verification fails → log error, continue to next account.

**THEN repeat A-D for the next account.**

This sequential approach is slower but prevents the mapping bug entirely.

### 5.3 Create Views

Add 3 views to the database:
- **By renewal** — table, sorted by Renewal Date ASC
- **At risk** — table, filtered: Health = at_risk OR critical
- **Board by health** — board, grouped by Health

### 5.4 Write Page Registry

Write `resources/notion-page-registry.md`:
```
DB={database-id}
DATA_SOURCE={data-source-id}
VIEW_DEFAULT={view-id}
VIEW_RENEWAL={view-id}
VIEW_AT_RISK={view-id}
VIEW_BOARD={view-id}
ACCOUNT:{name}={parent-page-id}
ACCOUNT:{name}:context={context-child-id}
ACCOUNT:{name}:manifest={manifest-child-id}
```

**Critical:** The registry must include child page IDs so the daemon can
fetch/update individual files without loading the parent.

---

## Phase 6 — Cadence + Scheduler

Write `resources/cadence.md` from template. Include DM monitor if enabled.

Default cadence.md:
```
work_hours: 7:00am - 6:00pm
work_days: monday, tuesday, wednesday, thursday, friday

weekdays at 7:30am → janitor
weekdays at 8:00am → morning-cycle
weekdays at 8:00am on mondays → expand morning-cycle to weekly
weekdays at 5:00pm → eod-cycle
fridays at 4:00pm → eow-cycle
1st monday at 8:30am → monthly-cycle
every 30m during work_hours → slack-dm-monitor

# Dispatch
dispatch_type: {dm|channel}
dispatch_target: {user_id|channel_id}
```

Confirm default times. Create Cowork recurring tasks:
- "Run scheduler every hour during work hours" (cadence evaluation)
- "Slack DM Monitor" at `*/30 * * * *` (if DM monitor enabled)

---

## Phase 7 — CLAUDE.md + Changelog

Generate CLAUDE.md from template with actual state (account list, MCP status, paths, dispatch config).
Seed `Ledger/changelog/{today}.md` with setup entry.

---

## Phase 8 — Verify

1. Fetch one Notion Context child page — confirm content matches local `_context.md`
2. Read one Slack account channel — confirm it resolves
3. Post test message to dispatch destination (DM or channel) — confirm delivery
4. Changelog exists with setup entry
5. All skill files installed at correct paths
6. Page registry has all accounts + child page IDs
7. DM monitor task exists (if enabled)

Print summary:
```
✅ CSA OS v4 Setup Complete

Accounts: {N} discovered from BQ, {N} confirmed
Slack channels: {N} verified, {N} unlinked
Notion: {N} account pages created ({N} × 2 child pages)
  DB: {db_id}
  Views: Default, By Renewal, At Risk, Board
Dispatch: Personal DM ({DISPATCH_TARGET})
Activity: {TELEMETRY_CHANNEL}
DM Monitor: {Active (30m) | Disabled}
MCPs: {list}

Post `briefing` in your DM to run your first morning cycle.
```

### Phase 9 — Post Install Event

Post the install event to the activity channel. Execute this — do not skip.

1. Read `resources/skills/personal/overrides.md` for TELEMETRY_CHANNEL and CSM_EMAIL
2. Call `slack_send_message` with channel=TELEMETRY_CHANNEL, message: a ```json code block containing:
```json
{"event":"csa_os:installed","properties":{"distinct_id":"{CSM_EMAIL}","time":{unix_ts},"csm_name":"{CSM_NAME}","version":"4.1.4","accounts":{N},"source":"{bq|manual|mixed}","mcps_connected":["{list}"],"notion_db":"{db_id}","slack_dispatch":"dm","telemetry_channel":"{TELEMETRY_CHANNEL}"}}
```
3. If the post fails, log the error — do not block setup completion.
