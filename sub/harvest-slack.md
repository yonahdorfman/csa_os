---
name: Harvest Slack
version: 2.0
last_updated: 2026-04-21
category: sub-skill
requires_mcp: [Slack]
inputs:
  - ACCOUNTS_PATH: "Base path to accounts"
  - SLACK_USER_ID: "Your Slack user ID"
  - CSM_TIMEZONE: "IANA timezone"
  - LOOKBACK_HOURS: "default: 24"
output: Structured signal list
invoke:
  - Called by assistant during harvest phase
  - "/harvest-slack"
---

# Harvest Slack

Scan Slack for signals. Return structured output. Don't update any files.

## Channel Discovery

Account Slack channels come from two sources (checked in order):
1. **BQ `account_metadata.slack_channel_id_c`** — pre-mapped channel IDs (most reliable)
2. **Account `_MANIFEST.md` Sources table** — manually added channels

Channel naming conventions at Mixpanel:
- Internal: `#at-{account-name}` (e.g., `#at-workday`, `#at-docusign`)
- External (shared): `#{account}-mixpanel` or `#mixpanel-{account}`
- Custom: anything linked in the account's `_MANIFEST.md` Sources table

If no channel is mapped, try searching for these patterns in order:
1. `at-{account_safe_name}`
2. `{account_safe_name}-mixpanel`
3. `mixpanel-{account_safe_name}`

## Steps

### 1. Build Channel List

For each account with a mapped Slack channel ID, read the channel.
Also search for DMs and mentions of <@{SLACK_USER_ID}>.

### 2. Scan Channels

For each account channel, read the last 30 messages in the lookback window.

If Slack MCP unavailable:
```
## Slack Harvest — {date}
**Status:** Slack MCP unavailable. Skipping.
```

### 3. For Each Message

Extract: channel name/ID, sender, text, timestamp, thread_ts, reactions, reply count.
Skip bot messages and automated notifications (Clari call summaries are valuable — keep those).

### 4. Classify

**Account:** matched from channel name (e.g., `#at-workday` → Workday)
**Reply status:** unreplied (no reply from {SLACK_USER_ID}), active_thread, resolved, noise
**Urgency:** critical, high, normal, noise
**Signal type:** risk, decision, approval, escalation, blocker, milestone, sentiment, renewal, general

### 5. Return Output

```markdown
## Slack Harvest — {date}

**Scanned:** {N} messages across {N} channels | **Actionable:** {N} | **Noise:** {N}

### Signals

#### {Account Name} — {urgency} — #{channel}
- **From:** {sender}
- **Message:** {summary}
- **Signal:** {interpretation}
- **Type:** {signal_type}
- **Reply status:** {unreplied|active_thread|resolved}

### Unreplied (needs response)
- #{channel}: {sender} asked about {topic} ({time} ago)

### Unmatched
- #{channel}: {summary}
```

## Notes

- Read-only. Never post messages during harvest.
- Gong (and legacy Clari) call summaries posted to Slack are high-value signals — extract action items from them. Gong is the primary call intelligence platform.
- DMs are sensitive — summarize, don't reproduce.
- Reactions 🚨 🔴 bump urgency.
