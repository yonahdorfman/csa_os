# Slack Harvest Rotation Cursor

last_batch_end_index: 0
last_run: 2026-07-08 13:xx GMT+3 (manual /harvest-slack run — priority-set only, rotation batch not yet started)
last_full_cycle_completed: —
batch_size: 15
total_accounts: 47

notes: Fresh cursor — no batches run yet. Priority order is computed fresh each tick
from Slack source `Last Fetched` dates (stalest first), so this index refers to a
position in that computed order, not a fixed account list.

## Hot Accounts (auto-detected high Slack activity)

Accounts here bypass rotation and are scanned every tick, same as manual pins in
`slack-priority-accounts.md`. An account is added when a scan finds message volume
above `hot_message_threshold` in the lookback window, or produces an
`unreplied_mention`. Each subsequent tick that still shows elevated activity resets
`ticks_remaining` to `hot_ttl_ticks`; otherwise it decrements. Remove the row when it
hits 0.

hot_message_threshold: 10
hot_ttl_ticks: 3

| Account | Reason Flagged | Detected | Ticks Remaining |
|---------|----------------|----------|------------------|
