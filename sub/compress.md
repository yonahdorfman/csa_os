---
name: Compress
version: 1.1
last_updated: 2026-07-06
category: sub-skill
description: >
  The intelligence step. Takes raw harvest output from all sources, classifies
  signals, diffs against existing context, and writes surgical patches to each
  account's _context.md. This is the only skill that modifies account context
  based on harvested signals.
requires_mcp: []
inputs:
  - ACCOUNTS_PATH: "Base path to accounts"
  - HARVEST_OUTPUT: "Combined output from all harvest-* skills"
  - CSM_NAME: "Your name"
  - CSM_TIMEZONE: "IANA timezone"
  - MAX_ACCOUNTS_PER_CYCLE: "default: UNLIMITED (process all accounts with signals)"
output: Summary of context patches applied
invoke:
  - Called by assistant after harvest phase
  - "/compress"
---

# Compress

Take raw signals from harvesters. Decide what's new. Write surgical updates to account context files.

## Principles

1. **Read before write.** Always read the current `_context.md` before modifying it.
2. **Append, never overwrite** in human-owned sections.
3. **Skip redundant signals.** If the context already contains this information, don't duplicate it.
4. **Date-stamp everything.** Every new entry includes today's date.
5. **Attribute the source.** "Via Gmail" or "Via Slack #channel" so the retro knows where it came from.
6. **Respect section ownership.** See below.
7. **`[x]` and `[~]` items are immutable.** Never re-open, remove, or modify them.

## Section Ownership (ENFORCED)

```
HUMAN-OWNED (above ## Sales Intelligence):
  Identity, Key Contacts, Current Health, Active Risk, Active Goal,
  Open Items, Recent History, V/R/G Log
  → Append-only. Never overwrite existing entries.
  → [x] closed and [~] archived items are IMMUTABLE.
  → Match duplicates by: date + account + first 50 chars.

MACHINE-OWNED (## Sales Intelligence and below):
  → Safe to overwrite on each refresh. Update timestamp.
```

BQ and SFDC are the SAME pipeline (BQ = SFDC + ETL lag + usage metrics).
Do not treat them as independent sources. Neither overrides a locally-authored entry.

## Steps

### 1. Parse Harvest Output

The assistant passes combined output from all harvest-* skills that ran.
Parse this into a flat list of signals, each with:
- account (matched or "unmatched")
- source (gmail, slack, calendar, drive, bq, notion)
- urgency (critical, high, normal, noise)
- signal_type (risk, decision, approval, escalation, blocker, milestone, sentiment, renewal, general)
- summary (one line)
- detail (full context)

### 2. Filter Noise

Remove signals classified as `noise`. Count them for the summary.

### 2.5. Account Processing Scope

**IMPORTANT:** Process ALL accounts with signals, not just a subset.

The default behavior is to process every account that has signals from the harvest.
DO NOT self-impose artificial limits like "top 10 by urgency" or "tool-budget reasons"
unless explicitly configured via MAX_ACCOUNTS_PER_CYCLE.

If MAX_ACCOUNTS_PER_CYCLE is set to a number (e.g., 10):
- Sort accounts by urgency: critical > high > normal
- Within same urgency, sort by: renewal date (soonest first), then ARR (highest first)
- Process only the top N accounts
- Log which accounts were skipped and why

If MAX_ACCOUNTS_PER_CYCLE is UNLIMITED (default):
- Process ALL accounts with signals
- This is the correct default behavior for a CSA OS managing a full book

**Rationale:** A CSA managing 47 accounts needs context freshness on all 47, not
just the top 10. Artificially limiting to 10 accounts means 66% of the book goes
stale (>14 days without refresh), which defeats the purpose of automated context
maintenance.

### 3. Group by Account

Group remaining signals by account name. For unmatched signals, file to `{ACCOUNTS_PATH}/Inbox/`.

### 4. For Each Account with Signals

#### 4a. Read Current Context

Read `{ACCOUNTS_PATH}/{account}/_context.md` in full.

#### 4b. Diff: What's New?

For each signal, check if the information is already in the context:
- Is this event/interaction already in Recent History? → skip
- Is this risk already in Active Risks? → update status if changed, skip if same
- Is this contact already in Key Contacts? → update if info changed, skip if same

Classify each signal as:
- `new` — genuinely new information, needs to be added
- `update` — modifies an existing entry (e.g., risk status changed)
- `redundant` — already captured, skip

#### 4c. Build Context Patches

The compress step produces RICH context — not thin one-liners. Every entry
should include what happened, why it matters, and where the information came from.

Compare:
- BAD: "2026-04-16: Data drop alert for project 2535275"
- GOOD: "2026-04-16: Event volume anomaly flagged in #at-yelp — Remy Elbez (internal) spotted project 2535275 with tons of anomalies and events going to zero in last 24h. Rich Mahon pinged Emmett. Emmett confirmed expected: Yelp's data is delayed 48 hours; confirmed it's likely biz_actions traffic. No customer-facing action needed. Source: Slack (#at-yelp)"

The good version tells you WHO flagged it, WHAT the root cause is, WHETHER
action is needed, and WHERE to find the original thread. The bad version
tells you nothing actionable.

**Recent History entries:**
```
- {YYYY-MM-DD}: {What happened — full context, not just a label. Include who
  was involved, what was discussed/decided, what the implications are, and
  what needs to happen next. End with source attribution.} Source: {Slack|Gmail|
  Calendar|Statisfy|DM note} ({#channel or thread reference})
```

**Active Risk entries (new):**
```
- **{YYYY-MM-DD}:** {Risk description} — {WHY this is a risk, what it could
  lead to, what the business impact is. Not just "churn risk" but "low switching
  costs — Mixpanel is abstracted above Redshift pipeline, technically replaceable
  with lower friction than a deeply embedded deployment."}
```

**Active Risk entries (update):**
Find the existing risk entry and append an update. Don't replace the original
analysis — add to it:
```
- **Update {YYYY-MM-DD}:** {what changed, new information, status shift} [via {source}]
```

**Open Items (new):**
Include urgency markers for items due this week or overdue:
```
- [ ] ⚠️ {description} — {owner} — {due date} — {status if in progress}
```

For overdue items, make the overdue status impossible to miss:
```
- [ ] ⚠️ {description} — {owner} — OVERDUE (due {date}, now {days} days late)
```

**Key Contacts (update):**
When a new contact is discovered, add them with title, role in the relationship,
email, and last-touched date. Don't just add a name — add context:
```
- William Schindhelm Georg — Unknown title — MCP Follow-Up contact
  (williamsg@yelp.com); accepted meeting Apr 10 — last touched: 2026-04-03
```

**Current Health (update):**
If signals indicate a health change, rewrite the health paragraph — don't
just append a line. The health section should always read as a current
assessment, not an accumulation of updates. Include the structural dynamics
that affect the account trajectory.

**Active Goal (update):**
If signals relate to a strategic goal, update progress. Goals should describe
the target state and the primary lever, not just "expand usage."

**Value/Risk/Goal Log (append-only):**
This section replaces the old Brief file. It's an append-only, dated log inside
`_context.md` that captures value delivered, risks flagged, and goal milestones.
Retros, briefings, and dashboards read from this section.

```
### Value/Risk/Goal Log

**{YYYY-MM-DD} | VALUE | {summary}**
- {what value was delivered, for whom, impact}
- Source: {retro|briefing|DM note|harvester}

**{YYYY-MM-DD} | RISK | {summary}**
- {risk description, business impact, mitigation status}
- Source: {retro|briefing|DM note|harvester}

**{YYYY-MM-DD} | GOAL | {summary}**
- {goal milestone, progress update, next step}
- Source: {retro|briefing|DM note|harvester}
```

Entries are newest-first, append-only. Never edit or delete old entries.
This is the audit trail that QBR narratives, risk dashboards, and goal
tracking reports parse at read time.

#### 4d. Apply Patches

**Write to Notion first, cache locally second.**

Local `_context.md` is a working snapshot, not the source of truth.
Your personal Notion hub is the source of truth.

For each account with patches:

1. Build the full updated `_context.md` content (current + patches applied):
   - Update the `Last Updated` header to today with timestamp
   - Update `Updated By` to the source (e.g., "EoD retro (automated)")
   - Update `Why Updated` with a specific summary of what changed
   - Update `Previous` with a one-line of what the last state was
   - Insert new entries at the TOP of each section (newest first)

2. **Write to Notion** — call `notion-sync` write mode:
   - Include page-state detection:
     - If the local context was pulled as empty/metadata-only in step 1 of the cycle:
       → Pass `first-write=true` to notion-sync (REPLACE mode — populate empty page)
     - Else:
       → Standard write (UPDATE mode — merge patches into existing content)
   - Update the Notion Context child page:
     - First write: full markdown body replaces empty page (REPLACE)
     - Subsequent: patches merged into existing content (UPDATE)
   - Update parent page properties if health/renewal changed
   - If Notion write fails → log error, retry next cycle, do NOT proceed to step 3
   - Log: `compress:wrote:{account}:{init|patch}`

3. **Cache locally** — write the same content to local `_context.md`
   - This keeps the local snapshot in sync for the rest of this cycle
   - But if step 2 failed, this step is skipped — we never have local-only context

### 5. Handle Unmatched Signals

For signals that couldn't be matched to an account:
- Write them to `{ACCOUNTS_PATH}/../Inbox/{YYYY-MM-DD}-unmatched.md`
- These get triaged by the janitor or manually by the user

### 6. Log to Changelog

For each account that was updated, append to the changelog:
```
## {HH:MM} — compress:context-update
- **Account:** {account_name}
- **Type:** context:updated
- **Detail:** {N} new signals, {N} updates, {N} redundant skipped
- **Sections touched:** {Recent History, Active Risks, etc.}
```

### 7. Return Summary

```markdown
## Compression Summary — {date}

**Input:** {N} signals from {sources}
**Filtered:** {N} noise
**Processed:** {N} actionable

### Context Patches Applied
| Account | New | Updated | Skipped | Sections |
|---------|-----|---------|---------|----------|
| Acme Corp | 2 | 1 | 3 | History, Risks |
| TechCo | 1 | 0 | 0 | History |

### Unmatched → Inbox
- {N} signals filed to Inbox for manual triage

### No Action Needed
- {Account names where all signals were redundant}
```

## Edge Cases

- **Empty harvest:** If all harvesters returned nothing or were unavailable, say "No signals to compress" and log it.
- **New account detected:** If a signal references an account that doesn't have a folder yet, file to Inbox with a note: "Possible new account: {name}. Create folder with /add-account."
- **Conflicting signals:** If two sources say different things about the same topic (e.g., Slack says "deal closed" but Gmail says "still negotiating"), include both with source attribution. Don't resolve conflicts — flag them for human review.
