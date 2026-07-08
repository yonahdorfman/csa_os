---
name: Harvest Calendar
version: 1.0
last_updated: 2026-04-21
category: sub-skill
requires_mcp: [Google Calendar]
inputs:
  - ACCOUNTS_PATH: "Base path to accounts"
  - CSM_TIMEZONE: "IANA timezone"
output: Structured event list with account matching
invoke:
  - Called by assistant during harvest phase
  - "/harvest-calendar"
---

# Harvest Calendar

Pull calendar events for the current week. Match to accounts. Flag RSVPs.

## Steps

### 1. Fetch Events

Query primary calendar: Monday 00:00 → Friday 23:59, current week, in {CSM_TIMEZONE}.
Include: title, start/end, attendees, location, video link, RSVP status.

If Calendar MCP unavailable, return status note and skip.

### 2. Match to Accounts

For each event, check attendees and title against known account names from `{ACCOUNTS_PATH}/_MANIFEST.md`. Also check `_MANIFEST.md` per account for known contacts.

### 3. Flag Actions

- `needs_rsvp` — your RSVP is "needsAction"
- `today` — event is today
- `prep_needed` — account meeting where _context.md was last updated >7 days ago
- `conflict` — overlapping events

### 4. Return Output

```markdown
## Calendar Harvest — Week of {date}

### Today ({day})
- {time} — {title} [{account or "internal"}]
  Attendees: {names}. {flags if any}

### This Week
- {day} {time} — {title} [{account}]

### Needs RSVP
- {title} on {day} — respond by {time}

### Account Meetings (prep notes)
- {account}: meeting on {day}. Context last updated {N} days ago. Review _context.md before call.
```
