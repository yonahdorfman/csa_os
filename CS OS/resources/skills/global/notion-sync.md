---
name: Notion Sync
version: 5.1
last_updated: 2026-07-21
category: sub-skill
requires_mcp: [Notion]
inputs:
  - ACCOUNTS_PATH: "Base path to local account snapshots"
  - NOTION_PAGE_REGISTRY: "Path to registry"
  - LEDGER_PATH: "Base path to Ledger"
  - MODE: "pull | write | reconcile | full"
  - ACCOUNT: "optional — single account name, or omit for all"
output: Sync summary with per-account status
invoke:
  - "/notion-sync"
  - "/notion-sync pull"
  - "/notion-sync write"
  - "/notion-sync write Pearson"
  - Called by assistant at every stage of every cycle
---

# Notion Sync v5

**Your personal Notion hub is the single source of truth.**
Local `_context.md` files are working snapshots — a cache, not the primary store.

## What v4 Got Wrong

v4 wrote only metadata properties (ARR, Health, Renewal) to Notion parent pages.
**It did not write the full markdown page body** from local `_context.md` files.
Result: Notion pages had empty bodies. The "source of truth" was empty.

## What v5 Does

Every write sends the **full markdown content** to the Notion Context child page body,
PLUS extracts metadata for the parent page properties. Every pull reads the full
Notion page body and overwrites the local file completely.

```
Notion Context child page (source of truth)
  body: full markdown (_context.md content — all sections)
  parent properties: Health, Renewal Date, Last Updated

Local _context.md (working snapshot / cache)
  exact copy of whatever is in Notion
```

---

## Mode: PULL (Notion → Local)

Read the Notion Context child page. Overwrite local `_context.md`.
This is how every cycle starts.

**Default scope is DELTA, not full-book.** Before pulling page bodies, query the
Accounts DB for pages whose `Last edited time` is newer than the last successful
pull, and pull only those. Everything else keeps its local snapshot. A full pull
of all ~47 page bodies is reserved for: first run, `reconcile` finding drift, or
an explicit `/notion-sync pull --full`. Pulling the whole book twice a day when
0–5 pages changed is the system's largest avoidable token cost.

### Steps

**1. Fetch Notion Context child page**

Read the page by ID from the registry (`ACCOUNT:{name}:context`).
Use `notion-fetch` or `notion-get-page` — whichever returns the full page body.
Request content in markdown format.

**2. Extract the page body**

The page body is the full markdown content. This should contain all sections:
Identity, Key Contacts, Current Health, Active Risk, Active Goal, Open Items,
Recent History, Value/Risk/Goal Log, Sales Intelligence.

If the page body is empty or very short (<20 lines):
- Log warning: `notion-sync:pull:warning — {account} Notion page body is empty or stub`
- Still write to local (the file should reflect whatever Notion has, even if it's thin)

**3. Write to local `_context.md`**

Overwrite the local file completely with the Notion page body.
Do NOT merge — Notion is the source of truth, local is just a cache.

**4. Also pull: parent page properties**

Read Health, Renewal Date, Last Updated from the parent page properties.
If these differ from what's in the markdown body, the properties are
authoritative for those fields (since they're filterable in the DB).

**5. Log**

```
[HH:MM] — notion-sync:pull
- Account: {account_name}
- Source: Notion Context page {page_id}
- Size: {line_count} lines
- Status: ✅ Pulled | ⚠️ Empty/stub
```

---

## Mode: WRITE (Local → Notion)

Write the full local `_context.md` markdown to the Notion Context child page body.
Also extract metadata and update parent page properties.
This is how compress and retros persist changes.

**Scope: only the accounts that were patched this cycle** (by compress, a retro, or
a DM note) — typically 1–10 accounts, never a "batch of all 47." An account whose
local file didn't change has nothing to write. And the write happens in the SAME
cycle as the patch — never log it as "scheduled for a later cycle" (see the
orchestrator's pending-writes check, added 2026-07-21 after exactly that deferral
dropped a day's writes).

### Page-State Detection

Before writing, check the Notion Context page body size:
- If body is empty or <50 chars (metadata-only stub) → **REPLACE mode** (initialization)
- If body has content (≥50 chars) → **UPDATE mode** (subsequent cycles)

This detection is critical because BQ-seeded pages start with empty bodies
(properties only). The first write must populate the full body. Subsequent
writes merge patches into existing content.

### Steps

**1. Read local `_context.md`**

Read the full file from `{ACCOUNTS_PATH}/{account}/_context.md`.
Verify it exists and is not empty.
If missing or empty → log error, return, do NOT write to Notion.

**2. Extract metadata from markdown**

Parse the local markdown to extract property values for the parent page:

- **Health:** Look for health indicators in Current Health section.
  Map to: `healthy`, `at_risk`, `critical`
- **Renewal Date:** Look for renewal date in Identity section.
  Parse to date format.
- **Last Updated:** Set to current timestamp.

If any extraction fails → log warning, set to "TBD", continue with the write.
Metadata extraction is best-effort — it should never block the content write.

**3. Resolve the page ID (CRITICAL — registry only)**

Read `{NOTION_PAGE_REGISTRY}` directly and find the exact line
`ACCOUNT:{account_name}:context={page_id}` (exact string match on account name,
including punctuation/casing as it appears in the registry).

- **Never** use `notion-search` to find a Context page ID. Search is semantic and can
  return Google Drive docs, SFDC links, or unrelated Notion pages instead of the
  Context child page — this already caused a silent write-skip that was misdiagnosed
  as a "registry gap" (2026-07-14, MyHeritage + Lemonade) when the registry entry
  existed the whole time.
- **Never** reuse a page ID copied from a previous write or from memory. A copy-paste
  error once assigned one account's ID to a different account's write and nearly
  corrupted its Context page (2026-07-02, GameStory Israel draft used BlueThrone's ID).
  Always re-read the registry fresh for each account being written.
- If no matching `ACCOUNT:{name}:context=` line exists after actually reading the
  file (not after a failed search) → this is a genuine registry gap. Log
  `notion-sync:write:error — {account} no registry entry`, skip the write, and do
  NOT guess or fall back to search.

**4. Verify the resolved ID before writing**

Fetch the page by the resolved ID and check its ancestor-path title matches
`{account_name}` exactly. If it doesn't match, treat this the same as a missing
registry entry — log the mismatch and stop; do not write to the wrong page.

**5. Detect page state**

Using the verified page from step 4:
- If body is empty or <50 chars → set mode = `REPLACE` (first write)
- If body has content ≥50 chars → set mode = `UPDATE` (patch write)

**6. WRITE to Notion Context child page (CRITICAL)**

**REPLACE mode (first write / empty page):**
- Write the FULL local `_context.md` markdown as the page body
- Every section, every line, every entry
- This populates an empty page for the first time
- Use `notion-update-page` with full content replacement

**UPDATE mode (subsequent writes / page has content):**
- Read the existing Notion page body
- Apply patches from compress into the existing content
- Write the merged result back to Notion
- This preserves human edits while adding daemon patches

In both modes:
- **Content format:** markdown
- Use `notion-update-page` with the page ID resolved and verified in steps 3-4

If Notion write fails:
- Log error: `notion-sync:write:error — {account} Notion write failed: {reason}`
- Return error
- **Do NOT proceed to step 7** — never cache locally if Notion write failed

**7. Update parent page properties**

After the Context child page body is written, update the parent page:
- Health = extracted value
- Renewal Date = extracted value
- Last Updated = now

Use `notion-update-page` with the parent page ID from registry.

**8. Cache locally (ONLY if Notion write succeeded)**

Pull the updated Notion page back to local to confirm they match.
Or simply: the local file is already current (we just wrote FROM it).
Either way, after this step, local and Notion should be identical.

**9. Log**

```
[HH:MM] — notion-sync:write:{init|patch}
- Account: {account_name}
- Target: Notion Context page {page_id}
- Mode: {REPLACE (first write) | UPDATE (patch)}
- Size: {line_count} lines
- Metadata: Health={health}, Renewal={renewal}
- Status: ✅ Written | ❌ Failed ({reason})
```

---

## Mode: RECONCILE

Calls `sub/reconcile.md` in check mode. See that skill for full spec.

---

## Mode: FULL

Runs pull for all accounts, then write for any that need it.
Used at session start to ensure local and Notion are aligned.

---

## Write Manifest (after source changes)

After add-account or source discovery:
1. Read the Notion Manifest child page (`ACCOUNT:{name}:manifest`)
2. Update the sources table
3. Write the full Manifest content back to the Notion page body
4. Cache to local `_MANIFEST.md`

## Ledger Push (after briefings, retros, monthly)

Push operational output to Notion Workspace Ledger:
1. Read the Workspace Ledger page ID from registry (`WORKSPACE:ledger`)
2. Create a child page under Ledger:
   - Title: `{type} — {date}` (e.g., "Morning Briefing — 2026-04-29")
   - Content: the full report markdown (full body, not just a title)
3. Log: `notion-sync:ledger-pushed:{type}`

---

## Creating New Account Pages

If a local account has no registry entry:

1. Create parent page in DB with properties (Health, Renewal, Last Updated)
2. Create Context child page with full `_context.md` markdown as body
3. Create Manifest child page with full `_MANIFEST.md` markdown as body
4. Process ONE account at a time: parent → record ID → children → record IDs → verify
5. Add all IDs to registry
6. Log: `notion-sync:created:{account}`

---

## Conflict Detection

If compress tries to write but Notion was edited by a human since last pull:

1. Read Notion's current content
2. Compare with what compress is trying to write
3. If Notion has changes compress doesn't have → merge:
   - Use Notion content as base
   - Apply compress patches on top
   - Write merged content to Notion
4. If merge conflicts (same section edited in both) → write conflict file to Ledger/triage/
5. Log: `notion-sync:conflict:{account}`

---

## Notion MCP Tool Reference

The Notion MCP tools needed for v5:

| Operation | Tool | Key Parameters |
|-----------|------|---------------|
| Read page body | `notion-fetch` or `getConfluencePage` | `pageId`, content format |
| Write page body | `notion-update-page` | `pageId`, content (markdown) |
| Create page | `notion-create-pages` | `parent`, `properties`, `content` |
| Update properties | `notion-update-page` | `pageId`, `properties` |

**Critical:** The write tool must support writing markdown as the page body,
not just updating properties. Test this on one page before running a full sync.

---

## Error Handling

| Failure | Action | Recovery |
|---------|--------|----------|
| Local file missing | Log error, skip Notion write | Next cycle: pull from Notion first |
| Notion write fails | Log error, skip local cache | Retry next cycle; local unchanged |
| Metadata extraction fails | Log warning, set "TBD", continue write | Manual fix or next BQ refresh |
| Page body empty on pull | Log warning, still write to local | Investigate — may be a new/stub page |
| Registry page ID invalid | Log error, skip account | Run `/reconcile check` to find broken entries |

**The golden rule:** If Notion write fails, do NOT update local.
This prevents orphaned local-only context.

---

## Output

```markdown
## Notion Sync — {date} ({MODE})

| Account | Operation | Lines | Status |
|---------|-----------|-------|--------|
| Workday | write | 128 | ✅ |
| Yelp | write | 170 | ✅ |
| DocuSign | write | 95 | ✅ |
| ATB | pull | 112 | ✅ |
| McDonald's | write | 45 | ❌ Notion API error |

Summary: {N} written, {N} pulled, {N} failed, {N} skipped
```
