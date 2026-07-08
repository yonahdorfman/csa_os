---
name: Harvest Gmail
version: 1.0
last_updated: 2026-04-21
category: sub-skill
description: >
  Scan Gmail inbox for unread messages. Classify by account and urgency.
  Returns structured signal array. Does NOT update context — that's the
  compress skill's job.
requires_mcp: [Gmail]
inputs:
  - ACCOUNTS_PATH: "Base path to accounts"
  - CSM_TIMEZONE: "IANA timezone"
  - LOOKBACK_HOURS: "How far back to scan (default: 24)"
output: Structured markdown signal list
invoke:
  - Called by assistant during harvest phase
  - "/harvest-gmail"
---

# Harvest Gmail

Scan Gmail for actionable signals. Return structured output. Don't update any files.

## Steps

### 1. Load Account Names

Read `{ACCOUNTS_PATH}/_MANIFEST.md` to get the list of known accounts.
Build a lookup table: account name → possible email domains, contact names.

### 2. Load Gmail MCP Tools

Before checking availability, always attempt to load Gmail tools via ToolSearch:
- Call ToolSearch with query `select:mcp__09819049-2468-459a-9a08-5f9eb41cf91f__search_threads,mcp__09819049-2468-459a-9a08-5f9eb41cf91f__get_thread`
- Gmail tools are deferred by default and will not be available without this step
- Only skip if ToolSearch returns no results or the tools fail to load

If Gmail MCP is genuinely unavailable after the load attempt, return:
```
## Gmail Harvest — {date}
**Status:** Gmail MCP unavailable. Skipping.
```

### 3. Search Inbox

Search Gmail for unread messages in the lookback window:
- Query: `is:unread newer_than:{LOOKBACK_HOURS}h`
- Exclude: categories Promotions, Social, Updates, Forums
- Max results: 50

### 4. For Each Message

Read the message. Extract:
- **From:** sender name + email
- **Subject:** full subject line
- **Snippet:** first 200 chars of body
- **Date:** timestamp
- **Thread ID:** for grouping
- **Labels:** inbox categories

### 5. Classify Each Message

**Account matching:** Match sender domain or name against known accounts.
If no match, mark as `unmatched`.

**Urgency classification:**
- `critical` — subject/body contains: urgent, asap, immediate, critical, escalated, breaking. Or sender is a known executive/stakeholder.
- `high` — from a customer contact, contains action words (please, need, request, blocked, deadline)
- `normal` — customer-related but informational
- `noise` — automated notifications, newsletters, CC'd threads with no action needed

**Signal type:** risk, decision, approval, escalation, blocker, milestone, sentiment, renewal, general

### 6. Return Structured Output

```markdown
## Gmail Harvest — {date}

**Scanned:** {N} messages | **Actionable:** {N} | **Noise filtered:** {N}

### Signals

#### {Account Name} — {urgency}
- **From:** {sender}
- **Subject:** {subject}
- **Signal:** {one-line summary of what this means}
- **Type:** {signal_type}
- **Action:** {what the CSM should do, or "none"}
- **Thread:** {thread_id}

#### {Account Name} — {urgency}
...

### Unmatched
- {sender}: {subject} (couldn't match to an account)
```

## Notes

- This skill only READS. It never modifies Gmail (no marking as read, no sending).
- Output is consumed by the `compress` skill, which decides what to write to `_context.md`.
- If the lookback window returns zero results, say so — don't fabricate signals.
- Keep summaries factual. "Customer asked about timeline" not "Customer seems anxious."
