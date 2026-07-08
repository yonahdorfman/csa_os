---
name: Harvest Notion
version: 1.0
last_updated: 2026-04-21
category: sub-skill
requires_mcp: [Notion]
inputs:
  - ACCOUNTS_DB: "Notion Accounts database ID"
  - LOOKBACK_HOURS: "default: 24"
output: List of recently edited account pages
invoke:
  - Called by assistant during harvest phase
  - "/harvest-notion"
---

# Harvest Notion

Detect which account pages in Notion were edited by humans since last sync.
This is the "pull" detector — it tells the compress step what needs to be
pulled back to local.

## Steps

### 1. Query Accounts Database

Query the Accounts DB filtered by `last_edited_time > {LOOKBACK_HOURS}h ago`.
Sort by last_edited_time descending.

If Notion MCP unavailable, skip with status note.

### 2. For Each Recently Edited Page

- Read the page properties: Name, Health, Renewal date, Last updated
- Read the page body (at least the Context section)
- Note who edited it (last_edited_by)

### 3. Compare to Local

For each edited page, check if the local `_context.md` has a different `Last Updated` timestamp.
If Notion is newer → this account needs a pull.

### 4. Return Output

```markdown
## Notion Harvest — {date}

**Edited in Notion (last {LOOKBACK_HOURS}h):** {N} accounts

### Needs Pull (Notion is newer)
- {Account Name}: edited by {person} at {time}. Local last updated: {date}.

### Already Synced
- {Account Name}: timestamps match, no action needed.
```
