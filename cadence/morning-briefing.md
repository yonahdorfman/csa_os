---
name: Morning Briefing
version: 3.0
last_updated: 2026-04-27
category: cadence
inputs:
  - CSM_NAME, CSM_TIMEZONE
  - ACCOUNTS_PATH, LEDGER_PATH
  - HARVEST_OUTPUT: "Combined harvest output (passed by assistant)"
  - MODE: "daily | weekly"
output: Markdown briefing saved to Ledger + posted to Slack
invoke:
  - Scheduled: weekdays at configured time
  - "/briefing"
---

# Morning Briefing

Daily operational briefing. Posted to Slack dispatch (DM or channel).

## Slack Format — THE REFERENCE

The briefing posts to Slack as a single message. The format below is the
EXACT structure to follow. It was validated over 2+ weeks of daily use.
Do NOT deviate from this format — no walls of text, no repeated ARR numbers,
no generic action labels.

```
☀️ Morning Briefing — {Day}, {Month} {Date}
${total_arr} · {N} accounts · {N} renewals ≤90d · {N} open todos · {alerts if any}

{if any tasks were completed:}
✅ Completed Since Yesterday
• {Account} — {what was completed} ✓
• {Account} — {what was completed} ✓

🔥 First Things First
• {Account} — {specific situation}. {specific action needed today}.
• {Account} — {specific situation}. {specific action}.
• {Account} — {specific situation}. {specific action}.
• {Account} — {specific situation}. {specific action}.

📅 Today
{meetings or "No meetings scheduled — open {day}. Use for: {suggested tasks}."}

📋 Needs Action
1. {Account} — {specific task with context} (overdue/due date)
2. {Account} — {specific task}
3. {Person} (Slack) — {what you need from/to tell them}
4. {Account} — {specific task}

{if any tools are offline:}
⚠️ {Tool} offline — {workaround}.

Reply in this thread to act — e.g. "draft Yelp follow-up", "reply to Richter", "summarize DocuSign risk"
```

## Format Rules

1. **One line per account.** Each bullet is ONE account with ONE situation and ONE action. Never repeat an account across sections unless the issues are genuinely different.

2. **Specific, not generic.** "Check status today" is useless. "Check DPA thread status with Matt Rodgers" tells you exactly what to do. Include names of people, threads, documents.

3. **No ARR in every line.** Put total ARR in the header. Only mention an individual account's ARR if it's relevant to the urgency (e.g., renewal <60d).

4. **No walls of text.** Each bullet is 1-2 lines max. If you need more context, that's what `_context.md` is for. The briefing is a routing table, not a report.

5. **"Reply in this thread to act"** — the briefing is interactive. The CSA should be able to reply with "draft Yelp follow-up" and the daemon handles it.

6. **Calendar-aware.** Show today's meetings with account context. If no meetings, suggest how to use the open time based on what's overdue.

7. **People-aware.** If a teammate is OOO, say so and what it means. "Bobby OOO through May 4 — you're solo on 8 shared accounts."

## What NOT to Do

- Don't produce a "report" with headers, sub-headers, and paragraph explanations
- Don't include "ATTENTION" or "WARNING" sections — that's the old format
- Don't repeat information across sections (if it's in First Things First, don't also put it in Needs Action)
- Don't use block formatting with boxes and decorations — it looks ugly in Slack on mobile
- Don't pad with generic context ("Impact: $242K ARR" on every line)

## Steps

### 1. Read Current State

For each account, read `_context.md`. Extract:
- Overdue items (anything past its due date)
- Items due today or this week
- Active risks marked CRITICAL or high urgency
- Renewal dates <90 days out
- Health changes since yesterday

Read today's changelog for any overnight signals.
Read today's calendar for meetings.

### 2. Triage

Sort everything into three buckets:

**First Things First** (3-5 items max): Things that are ON FIRE or will be
if you don't act today. Renewals <30d, overdue deliverables to customers,
critical data issues, escalations.

**Today** (calendar): What's on the schedule. If nothing, suggest how to
use the time based on what's overdue.

**Needs Action** (numbered list): Specific tasks for today/this week.
Not a full open-items dump — just the ones that are actually due or blocking.

### 2.5. Verify Before You Flag

Before including any follow-up action item in First Things First or Needs Action,
verify it hasn't already been completed. For each candidate item:

**Asana verification (FIRST):**
- Read the registry from `CS OS/resources/asana-project-registry.md`
- For each account with potential action items, call `get_tasks` with:
  - `project_gid` = the account's GID from registry
  - Filter: tasks modified in the last 7 days (`modified_since`)
  - `opt_fields`: `name,completed,completed_at,notes,due_on,permalink_url`
- Check if any tasks were completed since yesterday:
  - If a task was marked complete AND matches a candidate item → flag it as DONE
  - Include completed tasks in a separate "✅ Completed" section of the briefing
- Check if open tasks in Asana match candidate items:
  - If an open task exists for the item → verify it's still actionable
  - If the task is overdue in Asana → escalate priority in the briefing

**Slack verification:**
- Read ALL channels linked to that account (both internal `#at-{name}` and external
  Slack Connect channels from the account's channel IDs)
- Do NOT cap message reads — use `oldest` set to 7 days ago and retrieve all messages.
  The default 30-message limit is not sufficient; threads may have more activity.
- Look for evidence the action was already taken: a message from the CSM addressing
  the item, a customer confirmation, a resolution note, etc.
- If evidence exists, mark the item DONE and exclude it from the briefing
  (or note it as completed if context is useful)

**Gmail verification:**
- Search for recent threads involving the account/contact
- When a matching thread is found, call `get_thread` on it (not just `search_threads`)
  to retrieve ALL messages in the thread — `search_threads` returns a capped list
  and will miss recent replies
- Look for a sent message from the CSM after the original trigger date that
  addresses the item
- If found, mark the item DONE

**Rule:** Only flag an item as needing action if you have confirmed — via Asana, Slack,
AND Gmail — that no action has been taken. When in doubt, check all three sources before
flagging. A false "still open" wastes the CSM's time more than a false "done".

### 3. Write the Briefing

Follow the Slack format above exactly. Write it as if you're texting a
colleague — direct, specific, no filler.

### 4. Save to Ledger and Notion

1. Save the full briefing to `{LEDGER_PATH}/briefings/{YYYY-MM-DD}-morning.md`
2. Push the briefing to Notion Workspace Ledger as a child page titled "Morning Briefing — {date}"

### 4.5. Create Asana Tasks

After the briefing is written but before posting to Slack, create Asana tasks
for items in "First Things First" and "Needs Action" sections.

1. Call the `/create-asana-tasks` sub-skill (from `sub/create-asana-tasks.md`)
2. Pass all action items from First Things First and Needs Action
3. The sub-skill will:
   - Check for duplicate tasks (skip if already exists)
   - Create tasks in the appropriate account projects
   - Set due dates based on urgency
   - Log what was created to the changelog

If Asana MCP is unavailable, skip this step and log a warning.

### 5. Post to Slack

This is the actual dispatch step. Execute it — do not skip.

1. Read `resources/skills/personal/overrides.md` for DISPATCH_TARGET and TELEMETRY_CHANNEL

**Post to personal DM (the CSA reads this):**
2. Call `slack_send_message` with:
   - channel: DISPATCH_TARGET
   - message: the complete briefing text from step 3, exactly as formatted
3. If this fails, log the error.

### 6. Post to Activity Channel (structured event)

The activity channel receives structured JSON — not formatted Slack messages.
This is the admin/debug feed AND telemetry feed combined.

1. If TELEMETRY_CHANNEL is set, call `slack_send_message` with:
   - channel: TELEMETRY_CHANNEL
   - message: a ```json code block containing:
```json
{"event":"csa_os:briefing_posted","properties":{"distinct_id":"{CSM_EMAIL}","time":{unix_ts},"type":"morning","csm_name":"{CSM_NAME}","accounts":{N},"critical_items":{N},"action_items":{N},"accounts_mentioned":{N},"dispatch_target":"{DISPATCH_TARGET}","version":"4.1.1"}}
```
2. If the post fails, log and continue — activity/telemetry never blocks.

### 7. Log to Changelog

```
## {HH:MM} — briefing:generated
- **Type:** briefing:generated
- **Detail:** {daily|weekly} — {N} first-things, {N} action items, {N} meetings
- **Dispatched:** {DISPATCH_TYPE} to {DISPATCH_TARGET}
```

## Weekly Mode (Monday or configured day)

Add after the daily format:

```
🎯 This Week
• {Priority 1 for the week}
• {Priority 2}
• {Priority 3}

📊 Book: {N} accounts | ${arr} | Healthy: {N} | At-Risk: {N}
```

Keep it to 3 priorities max. These come from last week's EoW retro carries.
