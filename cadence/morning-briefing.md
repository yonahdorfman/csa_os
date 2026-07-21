---
name: Morning Briefing
version: 3.1
last_updated: 2026-07-16
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

🔥 First Things First
• {Account} — {specific situation}. {specific action needed today}.
• {Account} — {specific situation}. {specific action}.
• {Account} — {specific situation}. {specific action}.
• {Account} — {specific situation}. {specific action}.
{...up to 10 total}

👀 Also Watching
{Account}, {Account}, {Account} — {one-line shared reason, e.g. "high renewal risk, didn't make today's cut"}

📅 Today
{meetings or "No meetings scheduled — open {day}. Use for: {suggested tasks}."}

📋 Needs Action
1. {Account} — {specific task with context} (overdue/due date)
2. {Account} — {specific task}
3. {Person} (Slack) — {what you need from/to tell them}
4. {Account} — {specific task}
{...every due/blocking item, no cap — do not stop at a round number}

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

Sort everything into four buckets:

**First Things First** (3-10 items max): Things that are ON FIRE or will be
if you don't act today. Renewals <30d, overdue deliverables to customers,
critical data issues, escalations.

**Also Watching** (names only, no full action lines): Any account carrying a
CRITICAL health flag, high renewal risk, or a genuinely urgent signal (e.g.
from the hygiene report's "Critical health / High renewal risk" list) that
didn't make the First Things First cap. The point is that nothing with a real
risk flag disappears silently just because the top list filled up — list the
account names with a shared one-line reason, not a full write-up per account.
If every flagged account fit in First Things First, omit this line entirely.

Also include here any account janitor Job 8 flagged as "possibly stale" this
morning (real activity newer than the local context file) that is *still*
unresolved after today's harvest+compress ran — i.e. harvest ran but didn't
find/patch anything for it. That's worth a manual look, since it means the
automated catch-up didn't close the gap Job 8 detected.

**Today** (calendar): What's on the schedule. If nothing, suggest how to
use the time based on what's overdue.

**Needs Action** (numbered list): Specific tasks for today/this week.
Not a full open-items dump — just the ones that are actually due or blocking.
**No cap.** List every item that is actually due or blocking, even if that's
more than 5 — do not truncate for length or aesthetics. If the list runs
long, that's a signal about the day, not a reason to hide items.

### 3. Write the Briefing

Follow the Slack format above exactly. Write it as if you're texting a
colleague — direct, specific, no filler.

### 4. Save to Ledger and Notion

1. Save the full briefing to `{LEDGER_PATH}/briefings/{YYYY-MM-DD}-morning.md`
2. Push the briefing to Notion Workspace Ledger as a child page titled "Morning Briefing — {date}"

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
- **Detail:** {daily|weekly} — {N} first-things, {N} also-watching, {N} action items, {N} meetings
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
