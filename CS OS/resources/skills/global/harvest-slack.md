---
name: Harvest Slack
version: 3.3
last_updated: 2026-07-21
category: sub-skill
requires_mcp: [Slack]
inputs:
  - ACCOUNTS_PATH: "Base path to accounts"
  - SLACK_USER_ID: "Your Slack user ID"
  - CSM_TIMEZONE: "IANA timezone"
  - LOOKBACK_HOURS: "default: 24"
  - ROTATION_BATCH_SIZE: "Non-priority accounts scanned per tick via stalest-first rotation, default: 15 — separate from and additive to the priority set, which is always scanned in full regardless of size"
output: Structured signal list
invoke:
  - Called by assistant during harvest phase
  - "/harvest-slack"
---

# Harvest Slack

Scan Slack for signals. Return structured output. Don't update any files.

**Note on scope:** This skill scans a rotating BATCH of accounts per invocation, not
the full book every time — see "Coverage Rotation" below. This is a deliberate fix for
a recurring failure mode: scanning all 47 accounts every tick repeatedly exhausted the
tool-call budget and caused the *entire* harvest to be skipped (0 signals), rather than
partial coverage. A bounded batch guarantees forward progress on every tick.

## Channel Discovery

**MANDATORY: For each account in the batch, take the UNION of these three sources.** Do not rely solely on the BQ-mapped internal channel ID — that only ever covers the internal `#at-{account}` channel:

1. **`resources/slack-channel-registry.md`** — the pre-resolved book-wide registry of channel/DM/group IDs per account (`{Account}:group:...=ID`, `{Account}:DM:...=ID`). Read it ONCE per run (it's one small file), not per-account. This is the cheapest and most complete ID source — use its IDs directly instead of re-searching Slack. If a scan discovers a source missing from the registry, append it.
2. **BQ `account_metadata.slack_channel_id_c`** — pre-mapped internal channel ID (most reliable for the internal channel only, not a substitute for the registry/manifest).
3. **Account `_MANIFEST.md` Sources table** — the full source list. Read every row where `Source` = `Slack`, regardless of `Type`. This includes, at minimum:
   - `channel` rows — both internal (`#at-{account}`) and external/shared (`#ext-mixpanel-{account}`, `#{account}-mixpanel`, `#mixpanel-{account}`) channels
   - `user` rows — 1:1 DMs with named contacts (read via `slack_read_channel` using the user_id as the channel_id)
   - `group` / `mpim` rows — multi-person DMs / group chats involving the account's contacts

**Never skip external channels, DMs, or group chats just because they aren't in BQ.** If the manifest lists it as `Active`, scan it every cycle, same as the internal channel.

Channel naming conventions at Mixpanel:
- Internal: `#at-{account-name}` (e.g., `#at-workday`, `#at-docusign`)
- External (shared): `#{account}-mixpanel` or `#mixpanel-{account}` or `#ext-mixpanel-{account}`
- Custom: anything linked in the account's `_MANIFEST.md` Sources table

If the manifest has no Slack rows at all (new/thin account), fall back to searching for these patterns in order, and if found, add the result to the manifest (see "Manifest Maintenance" below) so it's never missed again:
1. `at-{account_safe_name}`
2. `ext-mixpanel-{account_safe_name}`
3. `{account_safe_name}-mixpanel`
4. `mixpanel-{account_safe_name}`


## Coverage Rotation

Full-book scanning (all ~47 accounts) in one tick is what caused the budget-exhaustion
failure mode. Instead, each invocation covers one bounded batch (on top of the always-run
priority set — see "Priority Accounts" below) and hands off to the next:

1. **Read the cursor:** `resources/slack-harvest-rotation-cursor.md`. It stores a single
   shared `last_batch_end_index` (a position in the priority order computed in step 2,
   not a fixed account list) plus `last_run` and `last_full_cycle_completed`.
2. **Build the priority order:** all **non-priority-set** accounts from `Accounts/_MANIFEST.md`
   (i.e. excluding anything already scanned via the priority set this tick), sorted by
   their Slack sources' `Last Fetched` date, **stalest first**. Ties broken by ARR
   descending (bigger accounts get seen sooner when staleness is equal).
3. **Take the batch:** the next `ROTATION_BATCH_SIZE` accounts (default 15) starting from
   `last_batch_end_index` in that priority order, wrapping around to the top if it runs
   past the end of the list.
4. **Scan only that batch** using Steps 1-5 below.
5. **Update the cursor** at the end: set `last_batch_end_index` to the index right after
   the last account scanned, and `last_run` to now. If a full lap completed (index wrapped
   past the start), log that as a completed cycle.

`ROTATION_BATCH_SIZE` is a fixed, guaranteed allotment — it is never reduced by the size
of that tick's priority set (see "Priority Accounts" below for why). This guarantees every
non-priority account gets scanned at least once roughly every
`ceil(non_priority_account_count / ROTATION_BATCH_SIZE)` ticks (~3 ticks at the default
batch size for a book where ~12+ accounts are priority) instead of depending on whether
the priority set happened to leave room that day. If a single account's channels are
unusually large, `ROTATION_BATCH_SIZE` can shrink for that tick — note the reduction in
the output rather than silently truncating.

Exception: if the harvest is invoked explicitly as a full catch-up (not the regular hourly
tick — e.g. "/harvest-slack --full" or a dedicated catch-up session), ignore the cursor and
scan everything, sequentially, without a batch cap.

### Priority Accounts (bypass rotation — always scanned)

Some accounts shouldn't have to wait their turn. Two sources feed a combined,
de-duplicated priority set, checked fresh every tick:

1. **Manual pins** — every account listed in `resources/slack-priority-accounts.md`.
   CSM-maintained; add an account when it enters an active renewal negotiation, live
   escalation, or exec-engaged situation, remove it once that settles.
2. **Auto-priority by account state** — even without a manual pin, an account qualifies
   if its current `_context.md` shows: Renewal Risk = High/Critical, OR Contract End is
   lapsed with the renewal opp not Closed Won, OR ARR ≥ $100,000.
3. **Janitor Job 8 flags** — any account janitor.md's "Unprocessed Activity Check" flagged
   this morning as possibly-stale (real activity newer than the local context file) also
   bypasses rotation for this cycle, so the gap gets closed the same day it's detected
   instead of waiting for its rotation turn.
4. **Auto-detected "hot" accounts** — tracked in the `Hot Accounts` table of
   `slack-harvest-rotation-cursor.md`. After Step 2b's deep read, if an account's message
   count in the lookback window exceeds `hot_message_threshold` (default 10), or the scan
   produced an `unreplied_mention`, mark/refresh it as hot with `hot_ttl_ticks` (default 3)
   remaining. Decrement remaining ticks for hot accounts that don't re-qualify this cycle;
   drop the row at 0. This is what catches "this account just got noisy" even if nothing
   else about it looks high-priority on paper.

**Every tick, in order — two separate, uncapped-vs-fixed budgets, never shared:**
1. Compute the priority set = manual pins ∪ auto-priority-by-state ∪ Job 8 flags ∪ hot
   accounts.
2. Scan the entire priority set first, via Steps 1-5 below. **Uncapped, by design** — it
   is never subject to `ROTATION_BATCH_SIZE` or the rotation index, no matter how large it
   gets. A 46-account book routinely has 12+ simultaneously-priority accounts (high renewal
   risk, lapsed, ARR≥$100K); a fixed cap here would mean silently dropping coverage on the
   accounts that most need it.
3. Independently, run the stalest-first rotation (Coverage Rotation above) for a **full**
   `ROTATION_BATCH_SIZE` accounts, skipping any account already covered by the priority set.
   This batch is additive to step 2, not carved out of the same pool — the priority set's
   size never shrinks it.
4. Total accounts scanned this tick = priority set size + `ROTATION_BATCH_SIZE`. If the
   priority set is unusually large, say so in the output (`large priority set this cycle:
   {N} accounts`) — this is informational only now, not a budget problem, since it no
   longer costs the rotation batch anything. (Previously an oversized priority set forced
   the rotation batch to shrink toward zero — e.g. 2026-07-15 and 2026-07-16, when a
   17-19-account priority set consumed the entire `BATCH_SIZE=15` allotment and the
   rotation batch didn't run either day. That failure mode is now fixed by giving each
   budget its own line.)

## Steps

### 1. Build Channel List

For each account **in this tick's batch** (see Coverage Rotation above):
- Read `_MANIFEST.md` and collect every `Active` Slack source row (channel, external channel, user/DM, group/mpim) — see Channel Discovery above.
- Cross-check against BQ `slack_channel_id_c` for the internal channel ID as a fallback/verification.
- Also search for DMs and mentions of <@{SLACK_USER_ID}> not already captured by a manifest row.
- **1:1 DM cadence exception:** `user`-type rows (individual 1:1 DMs) are checked on a **monthly** cadence, not every harvest cycle — these are low-signal, mostly-dormant contacts and don't need per-cycle re-scanning. Before scanning a `user` row, check its `Last Fetched` date: skip it unless `Last Fetched` is empty (`—`) or more than ~30 days old. Channels and group/mpim rows are unaffected by this exception and are still scanned every cycle.

### 2. Scan Channels

For each Slack source collected in Step 1 (internal channel, external channel, DM, group chat alike — subject to the monthly-cadence exception for `user` rows above):

**Step 2a: Cheap peek (gate before deep read)**
- Read just the last ~5 messages of the channel
- Compare the newest message timestamp against the source's `Last Fetched` date
- If nothing is newer than `Last Fetched` → nothing changed since last scan. Update
  `Last Fetched` to today and move on — **skip the deep read and thread checks entirely**
  for this source. This is the main cost saver: quiet channels (which is most of them,
  most of the time) cost one small read instead of a full 30-message + per-thread scan.
- If there IS new activity → proceed to the deep read below.
- **Staleness check (channel/external channel/group/mpim rows only):** every cycle,
  regardless of whether Step 2a triggered a deep read, compare the *newest message's own
  timestamp* (not `Last Fetched`) against 30 days ago:
  - Newest message is 30+ days old → set `Status: Inactive` on that manifest row. The
    source is still peeked every cycle at full cadence — Inactive is a label for
    reporting/prioritization, not a scan-frequency cut (that distinction is reserved for
    the `user`-row monthly cadence exception below).
  - Newest message is within 30 days → ensure `Status: Active` (flip back immediately if
    it was previously marked Inactive).
  - This runs independently of the deep-read gate above: a channel can go a long stretch
    with nothing newer than `Last Fetched` and still correctly read as Active (e.g. last
    message was 12 days ago) — staleness is judged off actual message recency, not off
    when the account last happened to be scanned.

**Step 2b: Deep read (only for channels with new activity)**
- Read everything since that source's `Last Fetched` date — NOT a fixed `LOOKBACK_HOURS` window.
  Rotation means a non-priority account may not have been scanned for 3+ days; a fixed 24h
  lookback silently drops every message older than yesterday, which is exactly how messages
  get missed. `LOOKBACK_HOURS` only applies when `Last Fetched` is empty (first scan).
- Cap the deep read at ~100 messages per source; if the cap truncates, say so in the output
  (`{channel}: truncated at 100 msgs, oldest unread {date}`) instead of silently dropping.
- For a monthly `user`-row liveness check, the last 10 messages are enough.
- For `user`/DM rows, call `slack_read_channel` with the user's Slack ID as `channel_id`

**Step 2c: Check for threads (CRITICAL — don't miss side conversations)**
For each message surfaced in the deep read:
- Check if `reply_count` > 0 or `thread_ts` exists
- If YES → call `slack_read_thread(channel_id, thread_ts)` to get the full thread
- This captures side conversations that happen in thread replies, which the parent
  message alone would miss entirely
- **Efficiency optimization:** only fetch threads that have replies, and only within
  channels that already passed the Step 2a peek — never thread-check a quiet channel

**Step 2d: Prioritize unread threads**
- Within the batch, surface first any threads where you (<@{SLACK_USER_ID}>) were
  mentioned but haven't replied — these are the most likely actionable items

**Handling `user` rows on the monthly check:**
- If the DM channel returns zero messages, set `Status: Inactive` in the manifest with `Last Fetched` = today. Leave it Inactive until a future monthly check finds new activity, at which point flip it back to `Active`.
- If the DM channel returns messages, set/keep `Status: Active`, `Last Fetched` = today, and extract any genuinely new/actionable signal per the normal classification steps below.
- If `slack_read_channel` errors with `channel_not_found`, treat it the same as empty (set `Status: Inactive`, `Last Fetched` = today) and add a short note (e.g. "user not found") — do not retry the same ID next cycle.
- Rows with no captured Slack ID (`(unknown)`) can't be scanned via the API — leave as-is and flag for manual ID lookup rather than guessing.

If Slack MCP unavailable:
```
## Slack Harvest — {date}
**Status:** Slack MCP unavailable. Skipping.
```

### 2a. Manifest Maintenance (Last Fetched + Status)

After successfully scanning a Slack source, update that row's `Last Fetched` column in the account's `_MANIFEST.md` to today's date. Do this for every source type — internal channel, external channel, DM, group chat. If a source was discovered ad hoc (not previously in the manifest — e.g. found via `slack_search_channels`), add a new row for it with `Status: Active` and today's date in `Last Fetched`, so it is never missed in a future cycle.

Also write the `Status` column every cycle per the rules above: `Active`/`Inactive` from the
30-day staleness check for channel/external channel/group/mpim rows, or from the monthly
liveness check for `user` rows. `Status` reflects activity recency, not scan cadence —
an `Inactive` channel is still scanned on the same schedule as an `Active` one.

### 2b. Cursor Maintenance (Rotation + Hot Accounts)

**A run that does not read AND update the rotation cursor is not a completed harvest —
do not log `harvest:slack:completed` without it.** Do not improvise an ad-hoc "priority
accounts" list in place of the procedure above: the 2026-07-17→07-21 runs did exactly
that, the cursor sat at index 0 the whole time, and the ~30 non-priority accounts were
never scanned — the direct cause of missed Slack messages.

At the end of the run, update `resources/slack-harvest-rotation-cursor.md`:
- Set `last_batch_end_index` to the position right after the last rotation-batch account
  scanned (priority-set accounts don't move this index, only the rotation portion does).
- Set `last_run` to now; if the index wrapped past the end of the list, set
  `last_full_cycle_completed` to today.
- Update the Hot Accounts table: add/refresh rows for accounts that crossed
  `hot_message_threshold` or produced an `unreplied_mention` this tick (reset
  `ticks_remaining` to `hot_ttl_ticks`); decrement `ticks_remaining` for hot accounts not
  re-qualified; drop rows that hit 0.

### 3. For Each Message

**For parent messages:**
Extract: channel name/ID, sender, text, timestamp, thread_ts, reactions, reply_count.

**For thread replies (from slack_read_thread):**
Extract all messages in the thread:
- Each reply's sender, text, timestamp
- Check for mentions of <@{SLACK_USER_ID}>
- Check for your replies (track if you've responded)
- Identify if thread is still active or resolved

**Filtering:**
- Skip bot messages EXCEPT:
  - Gong call summaries (high value)
  - Clari call summaries (legacy, still valuable)
  - Asana/Linear task notifications (project context)
- Skip pure emoji reactions with no text
- Keep messages with customer questions, blockers, escalations

### 4. Classify

**Account:** matched from channel name (e.g., `#at-workday` → Workday)

**Reply status** (single field — applies to both plain messages and threads):
- `unreplied_mention` — you were @-mentioned but haven't replied (HIGH PRIORITY)
- `unreplied_question` — customer asked something, no response from you yet
- `active_thread` — ongoing back-and-forth, you've already participated
- `resolved` — thread/message concluded, no action needed
- `side_conversation` — discussion happened but doesn't require your input
- `noise` — automated or irrelevant

**Urgency:** 
- critical (🚨 reactions, "urgent", "asap", executive mentions)
- high (customer questions, blockers, deadline mentions, @mentions of you)
- normal (informational, updates)
- noise (automated, resolved threads, casual chat)

**Signal type:** risk, decision, approval, escalation, blocker, milestone, sentiment, renewal, general

### 5. Return Output

```markdown
## Slack Harvest — {date}

**Priority accounts scanned:** {N} ({pin_count} pinned + {auto_count} auto-priority + {job8_count} Job 8 flags + {hot_count} hot) — uncapped {, large priority set this cycle: {N} accounts, if notably above average}
**Rotation batch:** {N} of {non_priority_total} non-priority accounts (index {start}-{end}), separate from and additive to the priority set above
**Skipped via cheap peek:** {N} sources (no new activity) | **Deep-read:** {N} sources | **Threads checked:** {N}
**Scanned:** {N} messages | **Actionable:** {N} | **Noise:** {N}

### Signals

#### {Account Name} — {urgency} — #{channel}
- **From:** {sender}
- **Message:** {summary}
- **Signal:** {interpretation}
- **Type:** {signal_type}
- **Reply status:** {unreplied_mention|unreplied_question|active_thread|resolved|side_conversation}
- **Thread:** {link, if applicable}

### Unreplied Mentions (HIGH PRIORITY)
- #{channel} (thread): @you mentioned by {sender} re: {topic} — {time} ago

### Unreplied Questions
- #{channel}: {sender} asked about {topic} ({time} ago)

### Newly Hot Accounts (just started bypassing rotation)
- {Account Name}: {N} messages this window (threshold {hot_message_threshold}) — will stay priority for {hot_ttl_ticks} ticks

### Unmatched
- #{channel}: {summary}
```

## Notes

- Read-only with respect to Slack itself — never post messages during harvest. Does write
  to two local tracking files: each account's `_MANIFEST.md` (`Last Fetched` column) and
  `resources/slack-harvest-rotation-cursor.md` (batch position) — both are bookkeeping, not content.
- Gong (and legacy Clari) call summaries posted to Slack are high-value signals — extract action items from them. Gong is the primary call intelligence platform.
- DMs are sensitive — summarize, don't reproduce.
- Reactions 🚨 🔴 bump urgency.
