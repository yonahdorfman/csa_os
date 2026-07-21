---
name: Slack DM Monitor
version: 2.0
last_updated: 2026-04-22
category: cadence
requires_mcp: [Slack]
inputs:
  - SLACK_USER_ID: "Your Slack user ID"
  - ACCOUNTS_PATH: "Base path to accounts"
  - CSM_TIMEZONE: "IANA timezone"
  - LEDGER_PATH: "Base path to Ledger"
schedule: "every 60m during work_hours"
invoke:
  - Scheduled: every 60 minutes during work hours
  - "/dm-monitor"
---

# Slack DM Monitor

Reads your personal Slack DM for notes and context updates. Detects
structured content (EOD reflections, weekly notes, account updates)
and routes it to the right place. Confirms every action in a threaded
reply on the original message.

## Why This Exists

You're in a meeting, on a call, or just had a Slack exchange. Instead of
opening Cowork, you DM yourself:

> "Workday: Chad confirmed all events fix landing in May"

The monitor picks it up, files it, and replies in-thread confirming
exactly what was written and where.

## Message Types

The monitor classifies each DM into one of these types:

### 1. Account Note (default)
A note about a specific account.

**Detection:** Message starts with or contains a known account name.
```
"Workday: spoke with Chad, fix confirmed for May"
"Call with Pearson went well, Drew on board with MCP"
"docusign session replay POC starting next week"
```

**Action:** Append to `{account}/_context.md` → Recent History.

### 2. EOD Reflection
End-of-day thoughts, what happened today.

**Detection:** Message starts with or contains: `eod`, `end of day`, `today's wrap`,
`today i`, `today was`, `wrapping up`, `done for the day`.
```
"EOD: Workday call went well. ATB still waiting on Session Replay testing.
Need to follow up with Pearson re: CISO meeting. Good day overall."
```

**Action:**
- Parse for account references → append to each account's Recent History
- Append full reflection to `{LEDGER_PATH}/retros/{YYYY-MM-DD}-eod-notes.md`
- This gets consumed by the scheduled EoD retro as human input

### 3. Weekly Reflection
End-of-week or weekly planning notes.

**Detection:** Message contains: `eow`, `end of week`, `this week`, `weekly`,
`week in review`, `next week`, `friday wrap`.
```
"EOW: Big week. Workday upsell moving. DocuSign POC kicked off. ATB renewal
in negotiation — feeling good but budget is tight. Next week: focus on
Pearson CISO prep and OpenTable experimentation POC."
```

**Action:**
- Parse for account references → append to each account's Recent History
- Append full reflection to `{LEDGER_PATH}/retros/{YYYY-MM-DD}-eow-notes.md`
- Consumed by the scheduled EoW retro as human input

### 4. Risk Flag
Urgent risk or concern about an account.

**Detection:** Message contains: `risk`, `at risk`, `churn`, `escalate`,
`worried about`, `concern`, `red flag`, `heads up`.
```
"Heads up: ATB might push back on renewal pricing. Emily mentioned
budget pressure from the board."
```

**Action:**
- Append to `{account}/_context.md` → Active Risks as new dated entry
- Append to Recent History
- Urgency: high (included in next briefing)

### 5. Action Item / Reminder
A task or follow-up.

**Detection:** Message contains: `todo`, `to do`, `follow up`, `need to`,
`reminder`, `don't forget`, `action item`.
```
"TODO: send Menards the UUID migration guide by Friday"
"Reminder to follow up with OpenTable on experimentation requirements doc"
```

**Action:**
- Append to `{account}/_context.md` → Open Items
- Format: `- [ ] **[DUE {inferred date}]** {task} (Owner: {CSM_NAME}) [via DM]`

### 6. General Note (no account match)
Something that doesn't reference a specific account.

**Detection:** No account name found, and doesn't match structured types.
```
"need to prep for quarterly review next week"
"grab the latest adoption metrics for the team meeting"
```

**Action:** File to `Inbox/{YYYY-MM-DD}-dm-notes.md`

## Execution Steps

### 1. Read Recent DMs

Read messages from DM with `channel_id={SLACK_USER_ID}` since last cursor.
If no cursor, look back 30 minutes.

Include both top-level messages AND threaded replies. The user may reply
in a thread on a briefing or retro — those are valid inputs too.

### 2. Filter and Route

- Only messages FROM {SLACK_USER_ID}
- Skip emoji-only or file-only messages
- Skip bot replies (messages from the daemon itself)

**Command detection:** If the message contains any of the keywords below,
it is a COMMAND, not a note. Execute the corresponding skill file immediately.
Do not skip. Do not classify as a note. Do not hand off to another process.
Read the skill file and run it NOW.

| If message contains | Read and execute this skill file |
|---------------------|----------------------------------|
| `briefing`, `morning briefing`, `run briefing` | `cadence/morning-briefing.md` |
| `retro`, `eod`, `end of day` | `cadence/eod-retrospective.md` |
| `weekly`, `eow` | `cadence/eow-retrospective.md` |
| `monthly` | `cadence/monthly-consolidation.md` |
| `janitor`, `hygiene` | `sub/janitor.md` |
| `refresh {account}` | Pull + harvest + compress + write for that account |
| `refresh`, `refresh all` | Run the full morning cycle from `roles/orchestrator.md` |
| `sync`, `reconcile` | `sub/reconcile.md` in fix mode |
| `status` | Post system status (headline+thread) to DM |
| `help` | Post the command list to DM |

After executing the command:
1. Reply in thread on the original message confirming what was done
2. Continue to step 7 (telemetry) with interaction_type="command", command="{keyword}"

**If message does NOT match any command keyword:** continue to step 3 (classify as a note).

### 3. Classify Each Message

Run through the type detection in order:
1. Check for EOD/EOW keywords → type 2 or 3
2. Check for risk keywords → type 4
3. Check for action item keywords → type 5
4. Check for account name match → type 1
5. Default → type 6 (general/unmatched)

A message can be BOTH a structured type AND account-specific.
e.g., "EOD: Workday call went well" is type 2 (EOD) AND references Workday.

### 4. Write to Context

**For account notes (type 1):**
```
**{YYYY-MM-DD} | DM note [via Slack DM]**
- {message content}
```
→ Append to `{account}/_context.md` → Recent History

**For EOD reflections (type 2):**
- Extract each account mentioned → append one-liner to each account's Recent History
- Write full text to `{LEDGER_PATH}/retros/{YYYY-MM-DD}-eod-notes.md`

**For EOW reflections (type 3):**
- Same as EOD but write to `{LEDGER_PATH}/retros/{YYYY-MM-DD}-eow-notes.md`

**For risk flags (type 4):**
```
**{YYYY-MM-DD} | Risk flagged [via Slack DM] [ACTIVE]**
- {risk description}
- Source: DM note from {CSM_NAME}
```
→ Append to `{account}/_context.md` → Active Risks AND Recent History

**For action items (type 5):**
```
- [ ] **[DUE {date}]** {task} (Owner: {CSM_NAME}) [via DM]
```
→ Append to `{account}/_context.md` → Open Items

**For unmatched (type 6):**
→ Append to `Inbox/{YYYY-MM-DD}-dm-notes.md`

### 5. Confirm (threaded reply)

Reply **in a thread on the original message** using `thread_ts`.

**Format matches the dispatch system's emoji + header + bullets pattern:**

**Account note (type 1):**
```
📝 Filed to {account_name}

Context updated:
• Recent History: "{first 100 chars of note}"
{if also updated Open Items:}
• ✓ Open Item resolved: "{item}"
```

**EOD reflection (type 2):**
```
🌙 EoD Notes Captured

Accounts referenced: {list}
• {account}: "{one-liner}" → Recent History
• {account}: "{one-liner}" → Recent History

📄 Full reflection saved to Ledger/retros/{date}-eod-notes.md
Will be included in tonight's EoD retro.
```

**EOW reflection (type 3):**
```
📋 EoW Notes Captured

Accounts referenced: {list}
• {account}: "{one-liner}" → Recent History
• {account}: "{one-liner}" → Recent History

📄 Full reflection saved to Ledger/retros/{date}-eow-notes.md
Will be included in this week's retro.
```

**Risk flag (type 4):**
```
⚠️ Risk Flagged — {account_name}

Active Risk added:
• **{YYYY-MM-DD} | {risk summary} [ACTIVE]**

Also added to Recent History.
🔴 Will be highlighted in next briefing.
```

**Action item (type 5):**
```
☑️ Action Item Added — {account_name}

Open Item:
• [ ] **[DUE {date}]** {task}

Owner: {CSM_NAME}
```

**Unmatched (type 6):**
```
📋 Filed to Inbox

Couldn't match to an account.
Tip: Start with the account name → "Workday: your note here"
```

### 6. Log to Changelog

```
## {HH:MM} — dm-monitor:{type}

- **Account:** {account_name or "book-level"}
- **Type:** {signal:new | retro:eod-input | retro:eow-input | risk:flagged | item:added}
- **Detail:** {summary}
```

### 7. Fire Telemetry

For each message processed in this run, do the following:

1. Read `sub/telemetry.md` to confirm the event format
2. Build the event JSON with these values from this run:
   - event: `csa_os:slack_interaction`
   - distinct_id: the CSM's email from overrides
   - interaction_type: `dm_note`
   - dm_note_type: the type you classified in step 3 (one of: account_note, eod, eow, risk, action, unmatched)
   - account: the account name you matched in step 3 (or null if unmatched)
   - thread_reply: false
   - version: current kit version from overrides
3. Post the JSON to Slack channel `C0B1ZU8QSHE` using `slack_send_message`
   - Wrap the JSON in a ```json code block
   - This is a real `slack_send_message` call, not a log entry
4. If the post fails, log the failure to changelog and continue to step 8
   - Telemetry failure never blocks the monitor

### 8. Update Cursor

Write last processed message timestamp to `resources/dm-monitor-cursor.md`.

## Configuration

In `cadence.md`:
```
every 60m during work_hours → slack-dm-monitor
```

In `overrides.md`:
```
DM_MONITOR_ENABLED=true
DM_MONITOR_INTERVAL=60m
```

## Graceful Degradation

- Slack MCP unavailable → skip, log
- No new messages → skip silently, update cursor
- Account match ambiguous → file to first mentioned, note ambiguity in thread reply
- Threading fails → fall back to single top-level summary

## Examples

---

**User DMs an account note:**
> "Pearson: Drew confirmed SDK block for web crawler shipping today. CISO meeting pushed to next week."

**Thread reply:**
```
📝 Filed to Pearson Global

Context updated:
• Recent History: "Drew confirmed SDK block for web crawler shipping today. CISO meeting pushed to next week."
• ✓ Open Item updated: [DUE 2026-04-22] Implement SDK block → In Progress
```

---

**User DMs an EOD reflection:**
> "EOD: Busy day. Workday call with Chad went well — all events fix confirmed for May. Sent ATB the PII deletion confirmation. Need to circle back with Yelp on the event drop issue tomorrow."

**Thread reply:**
```
🌙 EoD Notes Captured

Accounts referenced:
• Workday: "call with Chad, all events fix confirmed for May" → Recent History
• ATB Financial: "sent PII deletion confirmation" → Recent History
• Yelp: "need to circle back on event drop" → Recent History

📄 Full reflection saved to Ledger/retros/2026-04-22-eod-notes.md
Will be included in tonight's EoD retro.
```

---

**User flags a risk:**
> "Heads up: ATB might push back on renewal pricing. Emily mentioned budget pressure from the board."

**Thread reply:**
```
⚠️ Risk Flagged — ATB Financial

Active Risk added:
• **2026-04-22 | Budget pressure on renewal [ACTIVE]**
  Emily mentioned board-level budget pressure. May push back on pricing.

Also added to Recent History.
🔴 Will be highlighted in next briefing.
```

---

**User adds a reminder:**
> "TODO: send Menards the UUID migration guide by Friday"

**Thread reply:**
```
☑️ Action Item Added — Menards

Open Item:
• [ ] **[DUE 2026-04-25]** Send UUID migration guide (Owner: Emmett Faricy)
```

---

**User DMs something unmatched:**
> "need to prep for quarterly review next week"

**Thread reply:**
```
📋 Filed to Inbox

Couldn't match to an account.
Tip: Start with the account name → "Workday: your note here"
```
