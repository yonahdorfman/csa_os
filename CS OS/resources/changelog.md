# CSA OS Changelog

## 2026-07-19

### 12:07 — dm-monitor:risk_flag
- **Account:** Tabnine (Previously Codota)
- **Type:** risk:flagged
- **Detail:** Partial churn flagged — account reducing contact size despite ongoing renewal negotiations. Filed to Active Risks + Recent History. Will be highlighted in next briefing.

### 12:07 — dm-monitor:unmatched
- **Account:** Book-level (no match)
- **Type:** signal:general
- **Detail:** Add-account request for Wilma, Island, Fireblocks — accounts not in Notion registry. Filed to Inbox/2026-07-19-dm-notes.md pending registry update.

## 2026-07-20

### 09:36 — dm-monitor:BLOCKED
- **Status:** Slack MCP Authentication Required
- **Type:** system:auth_failure
- **Detail:** Scheduled DM monitor run blocked — Slack MCP requires OAuth authentication unavailable in non-interactive context. User must authenticate via claude.ai connector settings or `claude mcp` command in interactive session. Will retry at next 30m interval.

## 2026-07-21

### 15:06 — dm-monitor:no-op
- **Account:** Book-level (no match)
- **Type:** signal:none
- **Detail:** 3 messages found since cursor (harvest catch-up 13:04, hygiene report 13:01 + thread reply, morning briefing 09:15) — all bot-generated daemon output ("Sent using @Claude" footer), not user notes. Skipped per bot-reply filter. No account notes, EOD/EOW reflections, risk flags, or action items to file. Cursor advanced.
