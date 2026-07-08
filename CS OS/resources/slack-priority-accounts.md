# Slack Priority Accounts

Accounts listed here are scanned in **every** harvest-slack tick, regardless of
where they sit in the rotation queue. Edit this list directly — add an account
when it enters a state that needs eyes-on-Slack every cycle (active renewal
negotiation, live escalation, exec engaged, churn risk). Remove it once the
situation settles back to normal cadence.

This list is combined with auto-detected "hot" accounts (see
`slack-harvest-rotation-cursor.md`) — an account only needs to qualify for
one of the two to bypass rotation.

## Auto-priority criteria (no manual action needed)

An account also auto-qualifies as priority, without being listed below, if
any of these are true as of its last `_context.md` refresh:
- Renewal Risk = High or Critical
- Contract End is lapsed and the renewal opp is not Closed Won
- ARR ≥ $100,000

## Manually Pinned Accounts

| Account | Reason | Added | Remove When |
|---------|--------|-------|-------------|
