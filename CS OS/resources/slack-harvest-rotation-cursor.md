# Slack Harvest Rotation Cursor

last_batch_end_index: 0
last_run: 2026-07-16 (explicit full catch-up — all 46 accounts scanned via /harvest-slack --full, dispatched as 6 parallel batches)
last_full_cycle_completed: 2026-07-16
rotation_batch_size: 15
total_accounts: 46

notes: Full-book catch-up completed 2026-07-16 — every account's Slack sources
(channel, external channel, group/mpim rows, and any user rows due for their monthly
check) were scanned, not just a rotation batch. Index reset to 0 since the cursor
was bypassed for this run per the "explicit full catch-up" exception in harvest-slack.md.
Next regular tick resumes normal rotation from index 0, stalest-first.

As of 2026-07-16, `rotation_batch_size` (formerly `batch_size`) is a fixed allotment
for the non-priority rotation only — it is additive to, not shared with, the priority
set, which is scanned in full every tick regardless of size. See harvest-slack.md's
"Priority Accounts" section for why (the 2026-07-15/07-16 priority-overrun incidents
that this change fixes).

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
