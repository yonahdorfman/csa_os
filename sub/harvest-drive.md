---
name: Harvest Drive
version: 1.0
last_updated: 2026-04-21
category: sub-skill
requires_mcp: [Google Drive]
inputs:
  - ACCOUNTS_PATH: "Base path to accounts"
  - LOOKBACK_HOURS: "default: 24"
output: List of recently modified Drive docs matched to accounts
invoke:
  - Called by assistant during harvest phase
  - "/harvest-drive"
---

# Harvest Drive

Detect recently modified Google Drive documents that belong to known accounts.

## Steps

### 1. Discover Account Drive Folders

Read each account's `_MANIFEST.md` for linked Drive folder IDs or URLs.
Build a lookup: folder ID → account name.

### 2. Check for Recent Changes

For each linked folder, search for files modified in the last {LOOKBACK_HOURS} hours.

If no Drive folders are linked for any account, or Drive MCP unavailable:
```
## Drive Harvest — {date}
**Status:** No Drive sources configured (or MCP unavailable). Skipping.
```

### 3. For Each Modified File

Extract: file name, type (doc/sheet/slide/pdf), last modified time, last modifier.

### 4. Return Output

```markdown
## Drive Harvest — {date}

**Modified files (last {LOOKBACK_HOURS}h):** {N}

### By Account
#### {Account Name}
- {filename} ({type}) — modified by {person} at {time}
- {filename} ({type}) — modified by {person} at {time}

### Unmatched (not in a linked folder)
- {filename} in {folder}
```

## Notes

- Read-only. Never modify Drive files.
- This harvester is optional — many setups won't have Drive folders linked.
- When a deck or QBR doc is updated before a call, this signal is valuable for prep.
