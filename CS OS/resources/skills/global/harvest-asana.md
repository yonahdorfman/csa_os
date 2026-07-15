---
name: Harvest Asana
version: 1.0
last_updated: 2026-06-14
category: sub-skill
requires_mcp: [Asana]
inputs:
  - REGISTRY_PATH: "CS OS/resources/asana-project-registry.md"
  - LOOKBACK_DAYS: "default: 7 (tasks due within next 7 days, or overdue)"
output: Structured list of open tasks per account
invoke:
  - Called by assistant during harvest phase
  - "/harvest-asana"
---

# Harvest Asana

Scan Asana for open tasks across all account projects. Return structured output. Don't update any files.

## Setup

Load project GIDs from `CS OS/resources/asana-project-registry.md`.
PORTFOLIO_GID = `1214047596875876`

If Asana MCP unavailable:
```
## Asana Harvest — {date}
**Status:** Asana MCP unavailable. Skipping.
```

## Steps

### 1. Load Registry

Read `CS OS/resources/asana-project-registry.md`. Extract all `ACCOUNT:{name}={project_gid}` pairs.

### 2. Fetch Incomplete Tasks Per Project

For each account project GID, call `get_tasks` with:
- `project_gid` = the account's GID
- Filter: incomplete tasks only (`completed=false`)
- `opt_fields`: `name,due_on,assignee.name,notes,custom_fields,permalink_url,modified_at`

Batch project fetches where possible to stay within rate limits.

### 3. Classify Each Task

For each open task, assess:
- **Overdue:** `due_on` is before today → flag as overdue
- **Due soon:** `due_on` within 7 days → flag as due soon
- **No due date:** flag separately (lower priority)
- **Urgency:** overdue > due_soon > no_date
- **Signal type:** infer from task name — action_item, follow_up, renewal, escalation, outreach, internal, general

Skip tasks with no name or automated/template placeholders (e.g., tasks named "New Task").

### 4. Return Output

```markdown
## Asana Harvest — {date}

**Projects scanned:** {N} | **Open tasks:** {N} | **Overdue:** {N} | **Due this week:** {N}

### Overdue

#### {Account Name}
- **[{task name}]({permalink_url})** — due {due_on} ({N} days overdue)
  {notes snippet if present}

### Due This Week

#### {Account Name}
- **[{task name}]({permalink_url})** — due {due_on}
  {notes snippet if present}

### No Due Date

#### {Account Name}
- **[{task name}]({permalink_url})**
  {notes snippet if present}

### Projects With No Open Tasks
{comma-separated list of account names}
```

## Notes

- Read-only. Never create, edit, or complete tasks during harvest.
- If a project returns an error (permissions, not found), note it and continue.
- Tasks with no assignee are still surfaced — don't filter them out.
- Cap "No Due Date" section at 3 tasks per account to avoid noise. If more exist, note the count.

