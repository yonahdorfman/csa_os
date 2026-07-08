# Silence-Check Exempt Accounts

Accounts here communicate primarily through a channel CSA OS doesn't harvest
(WhatsApp, phone, in-person, a portal we don't have API access to, etc.). Without
this list, the silence checker would treat normal ongoing contact as if it had
gone quiet — a false alarm, not a real risk.

Listed accounts still get a "tracked-channel silence" number in the report (it's
useful context), but it is **never escalated as a risk flag** and never shown in
the urgent outreach list. Instead it appears once, under "Exempt — Verify
Manually," as a periodic nudge to confirm the untracked relationship is still
healthy.

Add an account here the moment you notice its main contact channel isn't one
CSA OS reads (Gmail, Slack, Calendar, Gong/Clari, Notion). Remove it if that
changes (e.g., they move a conversation into a tracked Slack channel).

## Exempt Accounts

| Account | Untracked Channel | Added | Notes |
|---------|-------------------|-------|-------|
| AT&T Israel | WhatsApp | 2026-07-07 | Primary contact happens in WhatsApp; Gmail/Slack will look silent even when the relationship is active. |
