# CSA OS v3 — Cadence Config
# Edit this file to change when things run.
# The orchestrator (the ONLY scheduled task) reads this file every hour
# and executes whatever is due. Do NOT create separate scheduled tasks
# for individual cadences.
# Format: <when> → <skill chain>

# ── Work boundaries ──
work_hours: 7:00am - 6:00pm
work_days: monday, tuesday, wednesday, thursday, friday

# ── Dispatch ──
dispatch_type: dm
dispatch_target: {SLACK_USER_ID}

# ── DM Monitor (context notes via Slack DM) ──
every 30m during work_hours → slack-dm-monitor

# ── Hygiene (before morning cycle) ──
weekdays at 7:30am → janitor

# ── Morning cycle ──
weekdays at 8:00am → notion-sync pull → harvest-all → compress → morning-briefing → notion-sync push → slack-dispatch post
weekdays at 8:00am on mondays → expand morning-briefing to weekly

# ── Evening cycle ──
weekdays at 5:00pm → notion-sync pull → eod-retrospective → notion-sync push → slack-dispatch post

# ── Weekly wrap-up ──
fridays at 4:00pm → notion-sync pull → eow-retrospective → notion-sync push → slack-dispatch post

# ── Monthly roll-up ──
1st monday at 8:30am → monthly-consolidation → notion-sync push → slack-dispatch post

# ── Notes ──
# harvest-all = run all available harvest-* skills in sequence
# dispatch_type: dm (post to your DM) or channel (post to a shared channel)
# dispatch_target: Slack user ID (for DM) or channel ID (for channel)
# DM monitor can be disabled by commenting out the line above
