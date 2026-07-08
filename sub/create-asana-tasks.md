---
name: Create Asana Tasks from Briefing
version: 1.0
last_updated: 2026-07-02
category: sub-skill
requires_mcp: [Asana]
inputs:
  - REGISTRY_PATH: "CS OS/resources/asana-project-registry.md"
  - BRIEFING_ITEMS: "Array of action items from briefing (morning or EOD)"
output: Created tasks in Asana with deduplication
invoke:
  - Called by assistant after briefing generation
  - "/create-asana-tasks"
---

# Create Asana Tasks from Briefing

Automatically create Asana tasks for action items mentioned in morning briefings
or EOD retrospectives. Checks for existing tasks to avoid duplicates.

## Setup

Load project GIDs from `CS OS/resources/asana-project-registry.md`.

If Asana MCP unavailable:
```
## Asana Task Creation — {date}
**Status:** Asana MCP unavailable. Skipping task creation.
```

## Steps

### 1. Load Registry

Read `CS OS/resources/asana-project-registry.md`. Extract all `ACCOUNT:{name}={project_gid}` pairs.

### 2. Parse Briefing Items

Extract action items from the briefing that should become Asana tasks.
Look for items in:
- **First Things First** section (morning briefing)
- **Needs Action** section (morning briefing)
- **Tomorrow** section (EOD retrospective)
- **Carrying** section (EOD retrospective)

For each item, extract:
- **Account name** (from the item text)
- **Task description** (the specific action needed)
- **Due date** (if mentioned, e.g., "overdue", "due today", specific date)
- **Priority** (infer from section: First Things First = high priority)

### 3. Match Account to Project

For each action item:
1. Extract the account name from the item text
2. Normalize the account name (strip spaces, lowercase for comparison)
3. Look up the account in the registry to find the `project_gid`
4. If no match found, log a warning and skip: `"Account '{name}' not found in registry"`

### 4. Check for Existing Tasks

Before creating a task, check if it already exists to avoid duplicates.

For each account project GID, call `get_tasks` with:
- `project_gid` = the account's GID
- Filter: incomplete tasks only (`completed=false`)
- `opt_fields`: `name,due_on,notes,permalink_url,created_at`
- Lookback: tasks created within the last 14 days

**Deduplication logic:**
- Compare the new task description with existing task names
- Use fuzzy matching: if 80%+ of words match, consider it a duplicate
- If a duplicate is found, skip creation and log: `"Task '{name}' already exists for {Account}"`

### 5. Create Tasks

For each non-duplicate action item, call `create_task` with:
- `project_gid` = the account's project GID from registry
- `name` = the task description (max 140 chars, truncate if needed)
- `notes` = additional context:
  ```
  Created from {briefing_type} briefing on {date}
  
  Context: {original briefing item text}
  
  Source: CSA OS v4.1.6
  ```
- `due_on` = derived from the item:
  - "overdue" or "due today" → today's date
  - "due this week" → Friday of current week
  - "due {date}" → parse the specific date
  - No date mentioned → 3 days from today (default)
- `assignee` = leave blank (Asana will use project defaults)

**Priority mapping (via custom fields if available):**
- First Things First items → High priority
- Needs Action items → Normal priority
- Tomorrow items → Normal priority
- Carrying items → High priority (overdue)

### 6. Return Output

```markdown
## Asana Task Creation — {date}

**Items processed:** {N} | **Tasks created:** {N} | **Duplicates skipped:** {N} | **Failed:** {N}

### Created Tasks

#### {Account Name}
- **[{task name}]({permalink_url})** — due {due_on}
  Created from {briefing_section}

### Duplicates Skipped

#### {Account Name}
- **{task description}** — matches existing task "[{existing_task_name}]({permalink_url})"

### Errors

#### {Account Name}
- **{task description}** — {error reason}

### Accounts Not Found in Registry
{comma-separated list of account names that couldn't be mapped to projects}
```

### 7. Log to Changelog

Append to today's changelog:
```
## {HH:MM} — asana:tasks_created
- **Type:** asana:tasks_created
- **Detail:** {N} tasks created, {N} duplicates skipped
- **Accounts:** {comma-separated list of accounts}
```

## Notes

- Never create tasks during harvest (read-only operation)
- Only create tasks when invoked after briefing/EOD generation
- Task names are truncated to 140 chars to avoid Asana API limits
- Deduplication prevents spam but isn't perfect — manual review may be needed
- If a project returns an error (permissions, not found), log it and continue
- Tasks are created without assignees — Asana project defaults will apply

## Integration Points

**Morning Briefing (`cadence/morning-briefing.md`):**
- After step 5 (Post to Slack), add:
  - Step 5.5: Call `/create-asana-tasks` with items from "First Things First" and "Needs Action"

**EOD Retrospective (`cadence/eod-retrospective.md`):**
- After step 8 (Post to Slack), add:
  - Step 8.5: Call `/create-asana-tasks` with items from "Tomorrow" and "Carrying"

## Example Task Creation

**Briefing item:**
```
• Lemonade — Check DPA thread status with Matt Rodgers
```

**Creates Asana task:**
- Project: Lemonade (GID: 1214129766921664)
- Name: "Check DPA thread status with Matt Rodgers"
- Notes: "Created from morning briefing on 2026-07-02\n\nContext: Check DPA thread status with Matt Rodgers\n\nSource: CSA OS v4.1.6"
- Due: 2026-07-05 (3 days from now)
