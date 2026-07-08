---
name: Add Account
version: 1.0
last_updated: 2026-04-22
category: sub-skill
requires_mcp: [Notion]
optional_mcp: [BigQuery, Slack]
inputs:
  - ACCOUNTS_PATH: "Base path to accounts"
  - NOTION_PAGE_REGISTRY: "Path to registry"
  - CSM_NAME: "Your name"
  - CSM_EMAIL: "Your email"
output: Local folder + Notion pages created, registry updated
invoke:
  - "/add-account"
  - "/add-account McDonald's"
  - "add an account"
  - "create account for {name}"
---

# Add Account

Creates a new account in both the local folder tree and Notion. Follows the
exact same structure as setup — parent page + 2 child pages in Notion, local
folder with _context.md and _MANIFEST.md.

## When to Use

- New account assigned to you
- Shared account you're co-owning (e.g., McDonald's)
- Account discovered in comms that wasn't in BQ

## Flow

### Step 1: Get Account Name

If invoked with a name (`/add-account McDonald's`), use it.
Otherwise ask: "What's the account name?"

Check if account already exists:
- Local: does `{ACCOUNTS_PATH}/{name}/` exist?
- Notion registry: does `ACCOUNT:{name}` exist in `{NOTION_PAGE_REGISTRY}`?

If exists → ask: "This account already exists. Did you mean to update it?"

### Step 2: Gather Account Info

Ask ONE question at a time. Accept blank/skip for anything the user doesn't know.

1. "What's the ARR?" (e.g., $500K, or skip)
2. "Renewal date?" (e.g., 2027-03-01, or skip)
3. "Who's the AE?" (name, or skip)
4. "Industry?" (e.g., QSR, E-commerce, Fintech, or skip)
5. "Is this a shared account? If so, who else is on it?" (co-owners)

### Step 3: Try BQ Enrichment (if available)

If BigQuery MCP is connected, attempt to enrich from BQ:

```sql
SELECT
  m.account_name, m.slack_channel_id_c, m.arr, m.industry, m.segment,
  m.account_owner, m.account_owner_email, m.sales_manager,
  m.health_score_vhs, m.health_score_adoption, m.sfdc_account_id, m.website,
  p.arr AS plans_arr, p.commercials_contract_end, p.tldr_health_rating,
  p.risk_renewal_level, p.sentiment_label, p.executive_summary,
  p.contacts_json, p.action_plan_json, p.risk_identified_risks_json,
  p.health_overall_assessment, p.gdoc_link
FROM `mixpanel-sa.sales_intelligence.account_metadata` m
LEFT JOIN `mixpanel-sa.sales_intelligence.account_plans_current` p
  ON m.account_name = p.account_name
WHERE LOWER(m.account_name) LIKE '%{account_name_lower}%'
LIMIT 3
```

If BQ returns results:
- Show what was found: "Found {name} in BQ with ARR ${X}, AE: {name}. Use this data?"
- If yes → merge BQ data with anything the user provided (user input overrides BQ)
- If multiple matches → ask user to pick the right one
- If no match → continue with what the user provided

If BQ unavailable or no match → continue with manual input only. No error, no block.

### Step 4: Discover Slack Channel (if Slack available)

If BQ returned a `slack_channel_id_c` → verify it with `slack_read_channel`.
If no BQ channel → search Slack for `at-{account_safe_name}`.
If found → confirm: "Found #at-{name} — use this channel?"
If not found → skip, note in Manifest that no channel is linked.

### Step 5: Create Local Folder

```
{ACCOUNTS_PATH}/{Account Name}/
├── _context.md           ← seeded (see below)
├── _MANIFEST.md          ← seeded with known sources
├── call-notes/
├── assets/
└── archive/
```

**Seed _context.md:**

If BQ data available → build full context using harvest-bq Step 3 logic
(Identity, Contacts, Health, Summary, Risks, Open Items, Sales Intel).

If manual only → seed with what the user provided:
```
# {Account Name}

**Last Updated:** {today}
**Updated By:** {CSM_NAME}
**Why Updated:** Account created

## Identity

| Field | Value |
|-------|-------|
| **Account Name** | {name} |
| **ARR** | {arr or "TBD"} |
| **Renewal Date** | {date or "TBD"} |
| **Primary AE** | {ae or "TBD"} |
| **CSA** | {CSM_NAME} |
| **Industry** | {industry or "TBD"} |

## Key Contacts

(No contacts yet — will be populated by harvesters)

## Current Health

New account. No health data yet.

## Recent History

**{today} | Account created [via /add-account]**
- Created by {CSM_NAME}
- {any notes from user input}
```

**Seed _MANIFEST.md:**
```
# {Account Name} — Manifest

## Sources

| Source | Type | Identifier | Status |
|--------|------|-----------|--------|
{if slack channel:}
| Slack | channel | #{name} ({id}) | Active |
{if BQ data:}
| BigQuery | sales_intelligence | account_plans_current | Active |
{if sfdc_id:}
| SFDC | account | [{id}](https://mixpanel.lightning.force.com/.../{id}/view) | Linked |
{if gdoc:}
| GDoc | account_plan | [Link]({url}) | Active |

## Collaborators

| Name | Role | Email |
|------|------|-------|
| {CSM_NAME} | CSA | {CSM_EMAIL} |
{if AE:}
| {AE name} | AE | {AE email} |
{if co-owners:}
| {co-owner} | Co-owner | — |
```

Update the root `{ACCOUNTS_PATH}/_MANIFEST.md` index table with the new account.

### Step 6: Create Notion Pages

**CRITICAL: Create ONE parent, record its ID, THEN create children under it.**

**Step 6a: Create parent page**
```
notion-create-pages:
  parent: { data_source_id: "{DATA_SOURCE_ID}" }
  pages: [{
    properties: {
      Name: "{account_name}",
      Health: "{health}",
      date:Renewal Date:start: "{date}",
      date:Last Updated:start: "{today}"
    }
  }]
```

Record the returned `page_id`. This is the parent.

**Step 6b: Create 2 child pages under that specific parent**
```
notion-create-pages:
  parent: { page_id: "{parent_page_id}" }   ← THE PARENT FROM 6a
  pages: [
    { properties: { title: "Context" }, content: "{_context.md content}" },
    { properties: { title: "Manifest" }, content: "{_MANIFEST.md content}" }
  ]
```

Record the 2 returned child page IDs.

### Step 7: Update Registry

Append to `{NOTION_PAGE_REGISTRY}`:
```
ACCOUNT:{Account Name}={parent-page-id}
ACCOUNT:{Account Name}:context={context-page-id}
ACCOUNT:{Account Name}:manifest={manifest-page-id}
```

### Step 8: Log and Confirm

Append to changelog:
```
## {HH:MM} — add-account:created

- **Account:** {Account Name}
- **Type:** account:created
- **Detail:** New account created. Source: {BQ + manual | manual only}.
  Notion pages: parent + 2 children. Slack: {#channel or "none"}.
```

**Post to Slack — execute these calls:**

1. Read `resources/skills/personal/overrides.md` for DISPATCH_TARGET and TELEMETRY_CHANNEL

**Personal DM (headline + thread):**
2. Call `slack_send_message` with channel={DISPATCH_TARGET}, message:
```
🆕 Account Added — {Account Name}
```
3. Record the returned message `ts`
4. Call `slack_send_message` with channel={DISPATCH_TARGET}, thread_ts={ts}, message:
```
📂 Local: {ACCOUNTS_PATH}/{name}/
📄 Notion: Parent + Context/Manifest
💬 Slack: {#channel or "not linked"}
📊 Source: {BQ enriched | Manual}

ARR: {arr or TBD} | Renewal: {date or TBD} | Health: {status or "unknown"}

Harvesters will start sweeping this account on the next cycle.
```

**Activity channel (structured event):**
5. If TELEMETRY_CHANNEL is set, call `slack_send_message` with channel=TELEMETRY_CHANNEL, message: a ```json code block containing:
```json
{"event":"csa_os:account_added","properties":{"distinct_id":"{CSM_EMAIL}","time":{unix_ts},"csm_name":"{CSM_NAME}","account":"{Account Name}","source":"{bq|manual|bq+manual}","has_slack_channel":{true|false},"shared":{true|false},"total_accounts":{N},"version":"4.1.1"}}
```

## For Shared Accounts

If the user indicated this is a shared account:
- Add co-owners to the Manifest Collaborators table
- Note in the _context.md header: "Shared account with {co-owner names}"
- The Notion page is accessible to anyone with workspace access
- Each CSA's daemon will independently harvest and compress for this account
- Conflicts (two CSAs updating the same context) are resolved by notion-sync's
  timestamp-based merge

## Notes

- This skill creates ONE account at a time. For bulk creation, use setup.
- If the user provides a Notion URL for an existing page, skip Notion creation
  and just add it to the registry.
- The account is immediately available to all harvesters on the next cycle.
- BQ enrichment is best-effort. The skill never blocks on BQ availability.
