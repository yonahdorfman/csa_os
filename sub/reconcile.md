---
name: Reconcile
version: 1.0
last_updated: 2026-04-29
category: sub-skill
requires_mcp: [Notion]
inputs:
  - ACCOUNTS_PATH: "Base path to local account snapshots"
  - NOTION_PAGE_REGISTRY: "Path to registry"
  - MODE: "check | fix | force-push | force-pull (default: check)"
output: Drift report + resolution actions taken
invoke:
  - "/reconcile"
  - "/reconcile check"
  - "/reconcile fix"
  - "/reconcile force-push Workday"
  - "/reconcile force-pull"
---

# Reconcile

Detects and fixes drift between local files and Notion. Run this when
something feels off, after a session crash, or as a periodic health check.

## Why Drift Happens

- A Cowork session crashed before the Notion write completed
- A human edited Notion directly and the daemon hasn't pulled yet
- A skill wrote locally but the Notion MCP was temporarily unavailable
- Setup created local files but Notion page creation failed partway through
- Two sessions ran against the same account (rare but possible)

## Modes

### `check` (default) — Report drift, don't fix

For each account in the registry:

1. Read local `_context.md` — extract `Last Updated` timestamp and content hash
2. Fetch Notion Context child page — extract `last_edited_time` and content
3. Compare:
   - **In sync** — timestamps within 5 minutes and content matches
   - **Notion ahead** — Notion was edited more recently (human edit or another session)
   - **Local ahead** — local was written but Notion wasn't updated (write failed)
   - **Diverged** — both changed since last sync (conflict)
   - **Missing local** — Notion page exists but no local file
   - **Missing Notion** — local file exists but no Notion page (registry gap)

Report:
```
## Reconcile Check — {date}

| Account | Status | Local Updated | Notion Updated | Delta |
|---------|--------|--------------|----------------|-------|
| Workday | ✅ In sync | Apr 29 10:15 | Apr 29 10:15 | — |
| Yelp | ⚠️ Local ahead | Apr 29 09:30 | Apr 22 08:00 | 7 days |
| DocuSign | ⚠️ Notion ahead | Apr 22 08:00 | Apr 28 14:00 | 6 days |
| ATB | 🔴 Diverged | Apr 29 09:00 | Apr 28 16:00 | both changed |
| McDonald's | ❌ Missing Notion | Apr 29 08:00 | — | no page |

Summary: {N} in sync, {N} drifted, {N} missing
```

### `fix` — Auto-resolve drift using section-zone merge

Reconcile respects the section ownership model. It does NOT do full-file overwrites.

**Section ownership boundary:** `## Sales Intelligence` header with `🤖` note.
Everything above = human zone. Everything below = machine zone.

**Resolution rules:**

| Status | Human Zone (above Sales Intel) | Machine Zone (Sales Intel) |
|--------|-------------------------------|---------------------------|
| **Notion ahead** | Merge: keep local entries + Notion-only entries, deduped. `[x]`/`[~]` items are immutable regardless of source. | Notion wins — overwrite local |
| **Local ahead** | Push local to Notion, pull back to confirm | Push local to Notion |
| **Diverged** | Merge both: keep all entries from both sources, deduplicated by date + first 50 chars. `[x]`/`[~]` always survive. For direct conflicts on the same entry, local wins. | Newer timestamp wins |
| **Missing local** | Pull from Notion → create local file | Pull from Notion |
| **Missing Notion** | Create Notion page from local content (parent + 2 children) | Create from local |

**Deduplication:** Match entries by date + account + first 50 characters.
If an entry exists in both local and Notion with the same match key, keep one copy.

**Immutability:** `[x]` closed and `[~]` archived items are NEVER removed or re-opened
by any automated merge, regardless of what Notion or upstream says.

Report what was done:
```
## Reconcile Fix — {date}

| Account | Was | Action | Result |
|---------|-----|--------|--------|
| Yelp | Local ahead | Pushed to Notion | ✅ In sync |
| DocuSign | Notion ahead | Pulled to local | ✅ In sync |
| ATB | Diverged | Merged (Notion base + local patches) | ✅ In sync |
| McDonald's | Missing Notion | Created page + 3 children | ✅ In sync |
```

### `force-push {account}` — Overwrite Notion with local

Use when you KNOW local is correct and Notion is wrong.
Reads local `_context.md` and overwrites the Notion Context page entirely.
Also updates Brief and Manifest if local files exist.

```
/reconcile force-push Yelp
→ Local Yelp _context.md (128 lines) → Notion Context page. Overwritten.
```

### `force-pull {account}` — Overwrite local with Notion

Use when you KNOW Notion is correct and local is stale.
Reads Notion Context page and overwrites local `_context.md`.

```
/reconcile force-pull Yelp
→ Notion Yelp Context page → local _context.md. Overwritten.
```

### `force-push` / `force-pull` (no account) — All accounts

Applies to every account in the registry. Use with caution.

## When to Run

- **After a session crash:** `/reconcile fix` — recovers any writes that didn't make it to Notion
- **Starting a new session:** `/reconcile check` — see what's drifted since last session
- **After manual Notion edits:** `/reconcile fix` — pulls your edits to local
- **Weekly hygiene:** janitor runs `reconcile check` as part of Job 4 (structural hygiene)
- **Something feels off:** `/reconcile check` first, then decide

## Integration with Janitor

The janitor skill (Job 4: Structural Hygiene) should call `reconcile check`
and include drift findings in the hygiene report. If drift is found, the
janitor flags it but doesn't auto-fix — that's the human's call.

## Integration with Setup

After setup completes (all accounts created in both local and Notion),
run `reconcile check` as the final verification step.

## Logging

```
## {HH:MM} — reconcile:{mode}

- **Type:** reconcile:{check|fix|force-push|force-pull}
- **Detail:** {N} accounts checked. {N} in sync, {N} fixed, {N} conflicts.
```

**Post to Slack (for `fix`, `force-push`, `force-pull` only) — execute these calls:**

1. Read `resources/skills/personal/overrides.md` for DISPATCH_TARGET and TELEMETRY_CHANNEL

**Personal DM (headline + thread):**
2. Call `slack_send_message` with channel={DISPATCH_TARGET}, message:
```
🔄 Reconcile — {N} fixed, {N} conflicts, {N} in sync
```
3. Record the returned message `ts`
4. Call `slack_send_message` with channel={DISPATCH_TARGET}, thread_ts={ts}, message:
```
| Account | Was | Action | Result |
|---------|-----|--------|--------|
| {name} | {status} | {action taken} | ✅ or ❌ |

{if conflicts:}
⚠️ Conflicts filed to Ledger/triage/ — review needed
```

**Activity channel (structured event):**
5. If TELEMETRY_CHANNEL is set, call `slack_send_message` with channel=TELEMETRY_CHANNEL, message: a ```json code block containing:
```json
{"event":"csa_os:reconcile","properties":{"distinct_id":"{CSM_EMAIL}","time":{unix_ts},"csm_name":"{CSM_NAME}","mode":"{fix|force-push|force-pull}","fixed":{N},"conflicts":{N},"in_sync":{N},"version":"4.1.1"}}
```

`check` mode does NOT post — it's silent unless invoked interactively.

## Notes

- `check` is always safe — it reads but never writes
- `fix` uses safe defaults (Notion wins for pulls, merge for conflicts)
- `force-push` and `force-pull` are destructive — they overwrite without merging
- For shared accounts, `force-push` updates YOUR Notion page, which other CSAs' daemons will see on their next pull
