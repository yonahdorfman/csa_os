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
  - LOOKBACK_HOURS: "How far back to scan (default: 72)"
output: Structured markdown signal list
invoke:
  - Called by assistant during harvest phase
  - "/harvest-gmail"
---

# Harvest Gmail

Scan Gmail for actionable signals. Return structured output. Don't update any files.

## Steps

### 1. Load Account Domains

For each account in `{ACCOUNTS_PATH}`:
1. Read the account's `_MANIFEST.md` file
2. Extract the **Domains** section to build a domain → account mapping
3. Store only: `{domain: account_name}` (lightweight lookup table)

### 2. Search Inbox

Before checking availability, always attempt to load Gmail tools via ToolSearch:
- Call ToolSearch with query `select:mcp__09819049-2468-459a-9a08-5f9eb41cf91f__search_threads,mcp__09819049-2468-459a-9a08-5f9eb41cf91f__get_thread`
- Gmail tools are deferred by default and will not be available without this step
- Only skip if ToolSearch returns no results or the tools fail to load

**Step 2a: Search for threads**
Use `search_threads` to find threads in the lookback window:
- Query: `newer_than:{LOOKBACK_HOURS}h`
- Exclude: categories Promotions, Social, Updates, Forums
- Max results: 100

This returns basic info for each thread: sender, subject, snippet, thread_id, date.

**Step 2b: Quick domain check**
For each thread from search results:
1. Extract domain from sender email
2. Check if domain exists in the domain registry (from Step 1)
3. **If domain matches** → call `get_thread(thread_id)` to get full details
4. **If domain doesn't match** → use the search_threads summary only

This optimization saves compute by only fetching full thread details for customer emails.

If Gmail MCP is unavailable, return:
```
## Gmail Harvest — {date}
**Status:** Gmail MCP unavailable. Skipping.
```

### 3. For Each Thread

**For matched domains (from get_thread):**
Extract from the full thread data:
- **From:** sender name + email (from latest message in thread)
- **Subject:** full subject line
- **Body:** full message body (not just snippet)
- **Date:** timestamp of latest message
- **Thread ID:** for reference
- **Labels:** inbox categories
- **Message count:** number of messages in thread

**For unmatched domains (from search_threads summary):**
Extract from the summary:
- **From:** sender name + email
- **Subject:** full subject line
- **Snippet:** first ~200 chars
- **Date:** timestamp
- **Thread ID:** for reference

### 4. Classify Each Message

**Account matching:** Use domain-first matching strategy.

**Matching Logic (Domain-Only):**

1. **Extract Domain from Sender**
   - Parse sender email (e.g., `newperson@altshul.co.il` → `altshul.co.il`)
   
2. **Domain Lookup**
   - Check if domain exists in any account's Domains section
   - If found → match email to that account
   - If not found → proceed to unknown domain detection
   
3. **Unknown Domain Detection**
   - If no domain match, check if it could be a customer domain:
     - NOT in excluded list: gmail.com, outlook.com, hotmail.com, yahoo.com, mixpanel.com, etc.
     - NOT obviously automated: noreply@, no-reply@, notifications@, donotreply@, bounce@, etc.
   - If passes both checks → flag as **Potential New Customer Domain**
   - Otherwise → mark as `unmatched`

**No individual email storage** — only domain matching for lightweight performance.

**Urgency classification:**
- `critical` — subject/body contains: urgent, asap, immediate, critical, escalated, breaking. Or sender is a known executive/stakeholder.
- `high` — from a customer contact, contains action words (please, need, request, blocked, deadline)
- `normal` — customer-related but informational
- `noise` — automated notifications, newsletters, CC'd threads with no action needed

**Signal type:** risk, decision, approval, escalation, blocker, milestone, sentiment, renewal, general

### 5. Return Structured Output

```markdown
## Gmail Harvest — {date}

**Scanned:** {N} messages | **Actionable:** {N} | **Noise filtered:** {N} | **New Domains:** {N}

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

### New Domains to Review

- **{domain}** (from {sender name} <{email}>) — Subject: "{subject}"
  - Could be: {best guess at what company this might be}
  - Action: Review and add to appropriate account manifest if customer-related

### Unmatched
- {sender}: {subject} (couldn't match to an account)
```

## Notes

- This skill only READS. It never modifies Gmail (no marking as read, no sending).
- Output is consumed by the `compress` skill, which decides what to write to `_context.md`.
- If the lookback window returns zero results, say so — don't fabricate signals.
- Keep summaries factual. "Customer asked about timeline" not "Customer seems anxious."
