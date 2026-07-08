# Personal Overrides — Edit This File with Your Values

This file defines your identity, paths, and preferences across all CSA OS skills.

**Location:** `/resources/skills/personal/overrides.md`

**How it works:** Skills read this file at startup to resolve {PLACEHOLDER} values.

---

## Identity

```
CSM_NAME={Your Full Name}
CSM_TITLE={Your Title}
CSM_EMAIL={Your Work Email}
CSM_COMPANY={Your Company Name}
CSM_TIMEZONE={IANA Timezone}
SLACK_USER_ID={Your Slack User ID}
SLACK_DISPLAY_NAME={Your Slack Display Name}
CSM_MANAGER={Your Manager's Name}
CSM_TEAM={Your Team Name}
```

## Folder Paths

```
COWORK_ROOT=/Cowork
ACCOUNTS_PATH=/Cowork/Accounts
LEDGER_PATH=/Cowork/Ledger
SKILLS_PATH=/Cowork/resources/skills
PERSONAL_CONTEXT_PATH=/Cowork/Personal Context
```

## Notion

```
ACCOUNTS_DB={Notion Accounts Database ID}
WORKSPACE_PAGE_ID={Personal Workspace Page ID}
NOTION_PAGE_REGISTRY=/Cowork/resources/notion-page-registry.md
```

## Slack

```
## Slack Dispatch

```
DISPATCH_TYPE=dm
# Options: dm (personal DM, default) or channel (shared channel)

DISPATCH_TARGET={SLACK_USER_ID}
# Where briefings, retros, alerts go. The CSA reads these.
# If dm: your Slack user ID (same as SLACK_USER_ID above)
# If channel: the channel ID (e.g., C0123ABC456)

TELEMETRY_CHANNEL=C0B1ZU8QSHE
# Structured JSON event stream. All daemon activity posted here:
# cycle completions, briefing events, errors, installs, syncs.
# Every CSA's daemon posts here. Used to monitor the system.
# Set to empty string to disable.

DM_MONITOR_ENABLED=true
DM_MONITOR_INTERVAL=30m
# How often to check your DM for account notes. Set false to disable.
```

## MCP Availability

Set these based on what's connected. Skills check these before attempting MCP calls.

```
MCP_NOTION=true
MCP_SLACK=true
MCP_GMAIL=true
MCP_CALENDAR=true
MCP_DRIVE=false
MCP_BIGQUERY=false
```

## Harvester Config

```
HARVEST_LOOKBACK_HOURS=24
HARVEST_MAX_RESULTS=50
WEEKLY_BRIEFING_DAY=1
```

## Cadence Overrides

Leave blank to use defaults from resources/cadence.md.

```
MORNING_BRIEFING_TIME=
EOD_RETRO_TIME=
EOW_RETRO_TIME=
MONTHLY_TIME=
JANITOR_TIME=
```
