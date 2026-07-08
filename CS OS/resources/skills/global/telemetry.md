---
name: Telemetry
version: 2.0
last_updated: 2026-04-30
category: sub-skill
requires_mcp: [Slack]
inputs:
  - TELEMETRY_CHANNEL: "C0B1ZU8QSHE"
  - CSM_EMAIL: "Distinct ID for the user"
invoke:
  - Called by orchestrator after each cycle
  - Called by setup on install
  - Called by slack-dispatch on human interaction
---

# Telemetry

Lightweight usage tracking for CSA OS. Events are posted as structured JSON
to a dedicated Slack channel (`C0B1ZU8QSHE`). Downstream processing (Zapier,
webhook, etc.) is configured by the admin — the kit only writes to Slack.

**Telemetry Channel:** `C0B1ZU8QSHE`
**Distinct ID:** CSM email

## Architecture

```
Cowork:
  orchestrator → telemetry.md → posts JSON to C0B1ZU8QSHE via Slack MCP

Downstream (admin-configured, outside the kit):
  watches C0B1ZU8QSHE → routes events wherever needed
```

Zero install for CSAs. Slack MCP is already required for the daemon.

## How to Post

After each event, the orchestrator posts a message to `C0B1ZU8QSHE`
using `slack_send_message`. The message body is a JSON code block:

```
```json
{"event":"csa_os:cycle_completed","properties":{"distinct_id":"emmett.faricy@mixpanel.com","time":1714500000,"cycle":"morning","accounts":10,"signals":24,"version":"4.0.2"}}
```
```

One message per event. JSON code block format for parseability.

If multiple events fire in one cycle (e.g., cycle_completed + briefing_posted
+ notion_sync), post each as a separate message.

Telemetry is fire-and-forget. If the Slack post fails, log it and move on.
Never block a cycle on telemetry.

---

## Events

### 1. `csa_os:installed`

Fired once during `/setup-csa-os`.

| Property | Type | Description |
|----------|------|-------------|
| `distinct_id` | string | CSM email |
| `version` | string | Kit version |
| `accounts` | number | Number of accounts seeded |
| `source` | string | How accounts were discovered: "bq", "manual", "mixed" |
| `mcps_connected` | list | Which MCPs passed probe |
| `notion_db` | string | Accounts DB ID |
| `slack_dispatch` | string | "dm" or "channel" |

### 2. `csa_os:cycle_completed`

Fired at the end of every orchestrator cycle.

| Property | Type | Description |
|----------|------|-------------|
| `distinct_id` | string | CSM email |
| `cycle` | string | "morning", "eod", "eow", "monthly" |
| `accounts` | number | Number of accounts in the book |
| `signals` | number | Total signals harvested |
| `patches` | number | Context patches written |
| `notion_writes` | number | Notion pages updated |
| `notion_errors` | number | Notion write failures |
| `harvesters_run` | list | Which harvesters executed |
| `harvesters_skipped` | list | Which skipped |
| `duration_seconds` | number | Wall clock time for the cycle |
| `backfilled_days` | number | Days backfilled (EoD only) |
| `version` | string | Kit version |

### 3. `csa_os:briefing_posted`

Fired when a briefing/retro/monthly is posted to Slack.

| Property | Type | Description |
|----------|------|-------------|
| `distinct_id` | string | CSM email |
| `type` | string | "morning", "eod", "eow", "monthly", "alert", "status" |
| `dispatch` | string | "dm" or "channel" |
| `critical_items` | number | Items in First Things First (briefing only) |
| `action_items` | number | Items in Needs Action (briefing only) |
| `accounts_mentioned` | number | Distinct accounts referenced |
| `version` | string | Kit version |

### 4. `csa_os:slack_interaction`

Fired when the human interacts with the daemon via Slack.

| Property | Type | Description |
|----------|------|-------------|
| `distinct_id` | string | CSM email |
| `interaction_type` | string | "command" or "dm_note" |
| `command` | string | The command: "briefing","retro","refresh","status","help" (null for dm_note) |
| `dm_note_type` | string | DM classification: "account_note","eod","eow","risk","action","unmatched" (null for command) |
| `account` | string | Account name if matched |
| `thread_reply` | boolean | Did the human reply in a briefing/retro thread? |
| `version` | string | Kit version |

### 5. `csa_os:account_added`

Fired when `/add-account` completes.

| Property | Type | Description |
|----------|------|-------------|
| `distinct_id` | string | CSM email |
| `account` | string | Account name |
| `source` | string | "bq", "manual", "bq+manual" |
| `has_slack_channel` | boolean | Slack channel linked? |
| `shared` | boolean | Co-owned account? |
| `total_accounts` | number | New total |
| `version` | string | Kit version |

### 6. `csa_os:notion_sync`

Fired on every sync operation.

| Property | Type | Description |
|----------|------|-------------|
| `distinct_id` | string | CSM email |
| `mode` | string | "pull", "write", "reconcile" |
| `accounts_synced` | number | Pages successfully synced |
| `accounts_failed` | number | Pages that failed |
| `first_writes` | number | Pages initialized (REPLACE mode) |
| `conflicts` | number | Conflicts detected |
| `duration_seconds` | number | Time for sync |
| `version` | string | Kit version |

### 7. `csa_os:error`

Fired on any error that blocks a cycle or skill.

| Property | Type | Description |
|----------|------|-------------|
| `distinct_id` | string | CSM email |
| `skill` | string | Which skill errored |
| `error_type` | string | "mcp_unavailable", "notion_write_failed", "bq_timeout", etc. |
| `error_message` | string | First 200 chars |
| `account` | string | Account name if relevant |
| `cycle` | string | Which cycle was running |
| `version` | string | Kit version |

---

## When to Fire

| Trigger | Event |
|---------|-------|
| Setup completes | `csa_os:installed` |
| Orchestrator completes a cycle | `csa_os:cycle_completed` |
| Briefing/retro posted to Slack | `csa_os:briefing_posted` |
| Human sends command in DM | `csa_os:slack_interaction` (command) |
| DM monitor files a note | `csa_os:slack_interaction` (dm_note) |
| Human replies in a briefing thread | `csa_os:slack_interaction` (thread_reply: true) |
| `/add-account` completes | `csa_os:account_added` |
| Notion sync runs | `csa_os:notion_sync` |
| Any skill errors | `csa_os:error` |

## What This Tells You

**Install:** How many CSAs, what MCPs, how many accounts.
**Passive usage:** Cycle frequency, signals processed, Notion reliability.
**Human engagement:** `slack_interaction` — are they reading briefings? Posting DM notes? Which commands? `thread_reply: true` is the adoption signal.
**Health:** `error` events show what's breaking.

## Privacy

- No customer data in events (no context, no message content)
- Account names included (internal names only)
- CSM email as distinct_id (internal users only)
- Error messages truncated to 200 chars
