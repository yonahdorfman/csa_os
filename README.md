# CSA OS v3 — Cowork Context Daemon

A context daemon that runs inside Cowork. Harvests signals from Slack, Gmail, Calendar, Notion, Drive, and BigQuery. Compresses them into structured account context. Syncs bidirectionally with Notion. Posts operational briefings to Slack.

## What Changed from v2

- **Tighter skills.** Each skill has a single job, clear inputs/outputs, and explicit MCP requirements.
- **Expandable harvesters.** Every source (Slack, Gmail, Calendar, Notion, Drive, BQ) is its own skill file. Add new sources by dropping a new file in `sub/`.
- **Compression pipeline.** New `compress` skill that takes raw harvested signals and writes surgical context patches to `_context.md` — the intelligence step is explicit and separate from harvesting.
- **Local-first with Notion mirror.** Account tree lives locally in Cowork. Notion is the mirror, not the source of truth. Bidirectional sync resolves by timestamp.
- **Changelog as state store.** The orchestrator reads the changelog to know what's run. Retros read the changelog instead of re-scanning sources.

## What's in the Kit

```
csa-os-v3/
├── README.md                           ← you are here
├── setup/
│   ├── setup.md                        ← single entry point: /setup-csa-os
│   └── first-run.md                    ← what to expect
├── sub/                                ← harvesters + primitives (12)
│   ├── harvest-gmail.md                ← Gmail signal extraction
│   ├── harvest-slack.md                ← Slack signal extraction
│   ├── harvest-calendar.md             ← Calendar events + RSVPs
│   ├── harvest-notion.md               ← Notion page edit detection
│   ├── harvest-drive.md                ← Drive doc change detection
│   ├── harvest-bq.md                   ← BigQuery sales intelligence
│   ├── harvest-all.md                  ← runs all harvesters, combines output
│   ├── compress.md                     ← signal → context patch pipeline
│   ├── notion-sync.md                  ← bidirectional Notion sync + Ledger push
│   ├── changelog.md                    ← append-only activity log
│   ├── janitor.md                      ← freshness, validation, hygiene checks
│   └── add-account.md                  ← create new account (local + Notion)
├── roles/                              ← orchestration (2)
│   ├── orchestrator.md                 ← THE single scheduled task (hourly tick)
│   └── slack-dispatch.md               ← Slack command handler + output poster
├── cadence/                            ← scheduled outputs (6)
│   ├── morning-briefing.md
│   ├── eod-retrospective.md
│   ├── eow-retrospective.md
│   ├── monthly-consolidation.md        ← now its own file
│   └── slack-dm-monitor.md             ← DM note capture (every 30m)
└── templates/                          ← file templates (5)
    ├── _context-template.md
    ├── _manifest-template.md
    ├── overrides-template.md
    └── claude-md-template.md
```

## Folder Structure (After Setup)

```
/Cowork/
├── Accounts/
│   ├── _MANIFEST.md                    account index
│   └── {Account Name}/
│       ├── _context.md                 working memory (source of truth in Notion)
│       ├── _MANIFEST.md                sources + collaborators
│       ├── call-notes/
│       ├── assets/
│       └── archive/
├── Ledger/
│   ├── changelog/                      daily activity logs
│   ├── briefings/                      morning briefings
│   ├── retros/                         daily + weekly retros
│   └── dashboards/                     hygiene reports
├── Personal Context/
│   └── about-me.md
├── Global Context/                     pulled from external sources
├── Team Context/                       pulled from external sources
├── Inbox/                              unprocessed items
├── resources/
│   ├── skills/global/                  this kit's skills
│   ├── skills/personal/               your overrides
│   ├── cadence.md                     schedule config
│   └── notion-page-registry.md        Notion ID mapping
└── CLAUDE.md                          auto-generated routing table
```

## The Daemon Loop

Your personal Notion hub is the source of truth. Local files are working snapshots.

```
1. Pull from Notion        ← get latest state (including human edits)
2. Harvest signals          ← gmail, slack, calendar, drive, bq, mixpanel
3. Compress                 ← classify signals, build context patches
4. Write to Notion          ← apply patches (this is the write that matters)
5. Cache locally            ← snapshot for the rest of this cycle
6. Changelog append         ← record what happened
7. Post to Slack            ← briefing, alerts, summaries
```

## Getting Started

```
/setup-csa-os
```

Requires: Notion MCP + Slack MCP connected. Gmail, Calendar, Drive, BQ are optional.

## Slack Integration

**Default behavior (configured during setup):**

- Briefings, retros, and alerts post to your **personal Slack DM** (not a shared channel)
- **DM Monitor** runs every 30 minutes during work hours, watching your DM for account notes
- Post something like `Workday: spoke with Chad, fix confirmed for May` and it gets filed to the Workday context automatically

Both can be changed to a shared channel or disabled in `cadence.md`.

## Notion Structure

Each account in the Notion Accounts DB is a parent page with 3 child pages:

```
Accounts DB
└── Workday                 ← parent (DB properties: Health, Renewal, Last Updated)
    ├── Context             ← full account context (source of truth)
    └── Manifest            ← sources and collaborators
```

Reading Context doesn't load Manifest — saves tokens on every cycle.

## Context Onion

Inner layers override outer. More specific wins.

```
🌐 GLOBAL    ← pull only (company docs, product info)
👥 TEAM      ← pull only (playbooks, roster)
👤 PERSONAL  ↔ sync (your profile, preferences)
🏢 ACCOUNT   ↔ sync (per-account context)
📓 LEDGER    → push only (briefings, retros → Notion)
```
