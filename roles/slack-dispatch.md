---
name: Slack Dispatch
version: 3.0
last_updated: 2026-04-22
category: role
requires_mcp: [Slack]
inputs:
  - DISPATCH_TYPE: "dm | channel"
  - DISPATCH_TARGET: "Slack user ID (for DM) or channel ID (for channel)"
  - SLACK_USER_ID: "Your Slack user ID"
  - ACCOUNTS_PATH: "Base path to accounts"
invoke:
  - Called by orchestrator to post results and read commands
---

# Slack Dispatch

Posts results to and reads commands from the dispatch destination.
Supports both personal DM (default) and shared channel modes.

## Dispatch Target

Read from `cadence.md` or `overrides.md`:
```
DISPATCH_TYPE=dm          # or "channel"
DISPATCH_TARGET=U04U7PBP5 # user ID for DM, channel ID for channel
```

**If DM mode:** Post messages using `slack_send_message` with `channel_id={SLACK_USER_ID}`.
Reading your DM = reading messages in the DM conversation.

**If channel mode:** Post to and read from the specified channel ID.

## Reading: Command Parsing

Read last 20 messages from the dispatch destination.
Only process messages from {SLACK_USER_ID}. Ignore bot posts.

### Commands

| Pattern | Action |
|---------|--------|
| `refresh {account}` | Harvest + compress for one account |
| `refresh all` / `refresh` | Full morning cycle |
| `sync` | Notion sync (full) |
| `briefing` / `morning` | Generate morning briefing |
| `retro` / `eod` | EoD retrospective |
| `weekly` / `eow` | EoW retrospective |
| `janitor` / `hygiene` | Run janitor checks |
| `status` | Post system status |
| `help` | Post command list |

### Context Inputs (DM mode)

In DM mode, non-command messages are treated as context notes.
These are handled by the `slack-dm-monitor` cadence skill, not by dispatch.
Dispatch only processes recognized commands.

### Context Inputs (Channel mode)

In channel mode, non-command messages are filed to account context:
- Match account name in message → append to `{ACCOUNTS_PATH}/{account}/_context.md` Recent History
- No match → file to `Inbox/`
- Reply in-thread confirming where it was filed

## Writing: Posting Results

### Posting Paradigm

**Morning briefings** post the full content as a single top-level message.
This is the one output you read start to finish every morning.

**Everything else** (retros, status, alerts, weekly, monthly) posts as:
1. **Top-level message** — one-line headline you can scan in your DM list
2. **Threaded reply** — the full content, read when you click in

This keeps your DM clean. You glance at the headline, click in if you need detail.

### Morning Briefing — FULL POST (top-level)

**See `cadence/morning-briefing.md` for the complete spec.**

```
☀️ Morning Briefing — {Day}, {Month} {Date}
${total_arr} · {N} accounts · {N} renewals ≤90d · {N} open todos · {alerts}

🔥 First Things First
• {Account} — {situation}. {action}.
• {Account} — {situation}. {action}.

📅 Today
{meetings or "No meetings — use for: {tasks}"}

📋 Needs Action
1. {Account} — {task} ({due/overdue date})
2. {Person} (Slack) — {task}

Reply in this thread to act — e.g. "draft Yelp follow-up"
```

### EoD Retro — HEADLINE + THREAD

**Top-level message:**
```
🌙 EoD — {N} accounts touched, {N} carrying, top priority tomorrow: {one-liner}
```

**Threaded reply (via `thread_ts` on the top-level):**
```
Done today:
• {Account} — {what you accomplished}
• {Account} — {what happened}

Carrying:
• {Account} — {what didn't get done and why}

Tomorrow:
1. {Top priority}
2. {Second priority}
3. {Third priority}

Context updates: {N} accounts patched. Changelog: Ledger/changelog/{date}.md
```

### EoW Retro — HEADLINE + THREAD

**Top-level:**
```
📋 Week in Review — {N} accounts active, {N} wins, {N} risks, focus next week: {one-liner}
```

**Thread:** Full weekly retro content (accomplished, open, risks, patterns, next week focus).

### Monthly Review — HEADLINE + THREAD

**Top-level:**
```
📊 Monthly Review — {month} — ${arr} ARR, {N} accounts, {N} at-risk, {N} renewals next 90d
```

**Thread:** Full monthly report (book overview, account health table, risk register, value delivered, focus areas).

### Status — HEADLINE + THREAD

**Top-level:**
```
📊 Status — synced {N} accounts, {N} stale, next: {cadence} at {time}
```

**Thread:**
```
Dispatch: {Personal DM | #channel_name}
DM Monitor: {Active (30m) | Disabled}
Last cycle: {time}
Synced: {N} accounts
Stale: {account names >7d}
Today: {N} signals, {N} updates
Next: {next cadence} at {time}
```

### Alerts — HEADLINE + THREAD

**Top-level:**
```
🔴 Alert — {account}: {one-line description of what's wrong}
```

**Thread:** Full context — what was detected, what the impact is, what to do.

Post alerts immediately without waiting for a scheduled cycle:
- Sync conflict detected
- MCP connection lost
- Stale context + renewal <60 days
- Critical event drop

### How to Post with Thread

1. Post the top-level message via `slack_send_message` → record the returned `ts`
2. Post the thread content via `slack_send_message` with `thread_ts` set to the `ts` from step 1

This is the same pattern the DM monitor uses for confirmations.
