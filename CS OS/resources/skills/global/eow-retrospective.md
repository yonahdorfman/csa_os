---
name: End-of-Week Retrospective
version: 3.1
last_updated: 2026-05-01
category: cadence
inputs:
  - ACCOUNTS_PATH, LEDGER_PATH, CSM_NAME, CSM_EMAIL, CSM_TIMEZONE
  - NOTION_PAGE_REGISTRY
invoke:
  - Scheduled: Thursday at configured time (via orchestrator)
  - "/weekly"
---

# End-of-Week Retrospective

Reads 5 days of changelog. Produces comprehensive weekly summary.
Writes V/R/G Log entries. Detects cross-account patterns.

## Steps

### 1. Pull Latest from Notion
### 2. Read the Week's Changelog (Sun–Thurs)
### 3. Read All Account Context
### 4. Write V/R/G Log Entries to Notion

For each account, append VALUE, RISK, GOAL entries for the week.
Section ownership enforced — append only, `[x]`/`[~]` immutable.

### 5. Detect Cross-Account Patterns

Look across accounts for: competitor evaluations, shared feature adoption,
response time trends, common blockers, best signal sources.

### 6. Generate Retro Content

```markdown
## Weekly Retro — Week of {date}

### Accomplished
- {Account}: {win or milestone}

### Still Open
- {Account}: {carrying into next week + why}

### Risks
- {Account}: {new or changed risk}

### Account-by-Account
- {Account}: {2-3 sentence week summary}

### Patterns
- {cross-account theme}

### Next Week
1. {Top priority}
2. {Second priority}
3. {Third priority}
```

### 7. Save to Ledger and Notion

1. Save to `{LEDGER_PATH}/retros/{YYYY-MM-DD}-eow.md`
2. Push to Notion Workspace Ledger as child page

### 8. Post to Slack (headline + thread)

1. Read `resources/skills/personal/overrides.md` for DISPATCH_TARGET
2. Call `slack_send_message` with channel={DISPATCH_TARGET}, message:
```
📋 Week in Review — {N} accounts active, {N} wins, {N} risks, next week: {top priority}
```
3. Record the returned `ts`
4. Call `slack_send_message` with channel={DISPATCH_TARGET}, thread_ts={ts}, message:
```
Accomplished:
• {Account} — {win}

Carrying:
• {Account} — {what + why}

Risks:
• {Account} — {risk summary}

Patterns:
• {cross-account theme}

Next week:
1. {priority}
2. {priority}

Full retro: Ledger/retros/{date}-eow.md
```

### 9. Post to Activity Channel (structured event)

1. Read `resources/skills/personal/overrides.md` for TELEMETRY_CHANNEL
2. If TELEMETRY_CHANNEL is set, call `slack_send_message` with channel=TELEMETRY_CHANNEL, message: a ```json code block containing:
```json
{"event":"csa_os:briefing_posted","properties":{"distinct_id":"{CSM_EMAIL}","time":{unix_ts},"type":"eow","csm_name":"{CSM_NAME}","accounts_active":{N},"wins":{N},"risks":{N},"version":"4.1.1"}}
```

### 10. Log to Changelog

```
## {HH:MM} — eow-retro:completed
- **Dispatched:** DM + activity channel
```
