# CSA OS v3 — First Run Walkthrough

What to expect when you run `/setup-csa-os` for the first time.

## Before You Start

**You need:**
- A Cowork session
- The v3 kit (uploaded archive)
- Notion MCP connected
- Slack MCP connected

**Optional (but recommended):**
- Gmail MCP connected
- Google Calendar MCP connected

**Time:** 20-30 minutes.

## What Setup Creates

### Local (in Cowork)

```
/Cowork/
├── Accounts/                    ← your account tree
│   ├── _MANIFEST.md             ← index of all accounts
│   └── {Each Account}/
│       ├── _context.md          ← working memory (hot cache)
│       ├── _MANIFEST.md         ← sources + collaborators
│       ├── call-notes/
│       ├── assets/
│       └── archive/
├── Ledger/                      ← operational output
│   ├── changelog/               ← daily activity logs
│   ├── briefings/               ← morning briefings
│   ├── retros/                  ← daily + weekly retros
│   └── monthly/                 ← monthly consolidations
├── Personal Context/            ← your profile
├── Global Context/              ← company-wide info (connected later)
├── Team Context/                ← team-specific info (connected later)
├── Inbox/                       ← unprocessed items
├── resources/
│   ├── skills/global/           ← the skill kit (18 files)
│   ├── skills/personal/         ← your overrides
│   ├── cadence.md               ← schedule config
│   └── notion-page-registry.md  ← maps accounts → Notion page IDs
└── CLAUDE.md                    ← auto-generated routing table
```

### Notion

**Accounts Database** with properties:
- Name (title), Health (select), Renewal date (date), Last updated (date)
- Views: All accounts, By renewal, At risk, Board by health
- Each account = one page with body sections: Context, Brief, Manifest

**Personal Workspace Page** with:
- Profile (from Personal Context)
- Ledger (empty — populated by briefings/retros)

### Slack

- Test message posted to your dispatch channel
- The daemon posts briefings, retros, alerts, and status here

## After Setup

### Test the Loop

Post `briefing` in your dispatch channel. The daemon will:
1. Pull from Notion (should be no-op since just synced)
2. Harvest signals from Gmail, Slack, Calendar (whatever's connected)
3. Compress: classify signals, diff against account context, write patches
4. Generate the briefing
5. Push updated context to Notion
6. Post the briefing to Slack

### Connect Sources to Accounts

Each account can have linked sources (Slack channels, Drive folders, etc).
Edit the account's `_MANIFEST.md` to add source entries:

```markdown
## Sources

| Source | Type | Identifier | Last Fetched |
|--------|------|-----------|-------------|
| #acme-corp | slack | C0123ABC | 2026-04-21 |
| Acme Drive | drive | folder-id | 2026-04-21 |
```

The harvesters read these to know which channels/folders to check per account.

### The Compression Pipeline

This is new in v3. The flow is:

```
Harvesters (read-only)     →   Compress (read + write)    →   _context.md
gmail-pulse returns signals     Reads current context           Gets surgical patches
slack-pulse returns signals     Diffs: what's new?              Dated, attributed entries
calendar-pulse returns events   Writes patches to sections      Append-only in dated sections
```

Harvesters never modify files. The compress skill is the only thing that writes to `_context.md` based on harvested signals. This separation makes it easy to add new harvesters without risking context corruption.

### How Sync Works

Every cycle starts with a Notion pull and ends with a Notion push:

```
Notion pull: if Notion page was edited by a human → overwrite local
  ↓
(harvest + compress happens on local files)
  ↓
Notion push: if local was updated → overwrite Notion page
```

Timestamps decide direction. If both changed since last sync → conflict file created in Ledger/triage/.

### Adding a New Harvester

Drop a new file in `resources/skills/global/sub/`:

```markdown
---
name: Harvest {Source}
requires_mcp: [{MCP Name}]
---

# Harvest {Source}

## Steps
1. Check MCP availability. Skip if unavailable.
2. Query the source for signals in the lookback window.
3. Classify each signal by account, urgency, type.
4. Return structured markdown output.

(Never modify files — output is consumed by compress.)
```

The assistant automatically picks up any `harvest-*.md` file in `sub/`.

### Slack Configuration (Defaults)

During setup you'll be asked two Slack questions:

**Q1: Where should briefings post?**
- **Your personal DM** ← default, recommended
- A shared #channel

**Q2: Monitor your DMs for account notes?**
- **Yes, every 30 minutes** ← default, recommended
- No, I'll update context manually

Both can be changed later in `cadence.md`.

**How the DM monitor works:** Post a note to your own Slack DM like:
> "Workday: Chad confirmed all events fix landing in May"

The monitor picks it up every 30 minutes, matches "Workday" to the account, and appends it to `_context.md` Recent History. It replies with "✅ Filed to Workday" so you know it worked.

This means you can update account context from your phone, during a call, or whenever — no need to open Cowork.

## Quick Reference

| Action | How |
|--------|-----|
| Run morning briefing | Post `briefing` in your DM (or dispatch channel) |
| Run retro | Post `retro` |
| Refresh one account | Post `refresh Acme Corp` |
| Refresh all accounts | Post `refresh all` |
| Check status | Post `status` |
| Sync with Notion | Post `sync` |
| Run hygiene checks | Post `janitor` |
| File a quick note | Post `Workday: spoke with Chad, fix confirmed` (DM monitor picks it up) |

## What Lives Where

| Data | Local | Notion |
|------|-------|--------|
| Account context | `Accounts/{Name}/_context.md` | Account page → Context child page |
| Account manifest | `Accounts/{Name}/_MANIFEST.md` | Account page → Manifest child page |
| Health/Renewal | Parsed from _context.md | DB properties (filterable) |
| Briefings | `Ledger/briefings/` | Workspace → Ledger |
| Retros | `Ledger/retros/` | Workspace → Ledger |
| Changelog | `Ledger/changelog/` | Not synced (local only) |
| Personal profile | `Personal Context/` | Workspace → Profile |
| Skills | `resources/skills/` | Not in Notion |
| CLAUDE.md | `{ROOT}/CLAUDE.md` | Not in Notion |
