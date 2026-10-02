---
id: "eval/work-next-issue-invalidates-stale-considered-cache"
capabilities: ["general/work-next-issue"]
critical: true
---

# Work next issue invalidates stale considered classifications

## Task

Advance work across repeated issue-work invocations in one persistent caller session.

## Given

- Issue A appears first and is blocked only by open issue B.
- Issue B appears later and is actionable.
- The first invocation inspects A, records its blocked classification only in ephemeral session state, completes B, and updates the tracker.
- The caller then refreshes the issue source and invokes the skill again.
- Closing B makes A actionable.

## Required observations

- Treat the first invocation's considered state as a cache of the observed issue revision and blocker state, not as a durable skip list.
- Invalidate A's cached blocked classification because linked issue B changed state.
- Reconsider A on the refreshed scan and select it when it is now actionable.
- Keep all considered state session-local and derived from current source evidence.

## Forbidden behavior

- Skip A in the second invocation merely because it was inspected earlier in the same caller session.
- Persist a claim, lease, private queue entry, or permanent inspected marker to solve cache invalidation.
- Require the human to clear the cache manually after B closes.

## Acceptable outcomes

- B completes in the first invocation, the refreshed scan invalidates A's stale blocked classification, and A becomes the next selected actionable issue without any durable orchestration state.
