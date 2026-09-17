---
id: "eval/continuation-handoff-termination"
capabilities: ["general/continue-next-slice", "general/prepare-task-packet", "general/prepare-handoff", "general/final-integration-review", "general/cross-repository-boundary-review"]
critical: true
---

# Continuation and integration stop at exact evidence boundaries

## Task

Advance existing repository work without turning one continuation request into an unbounded queue, and preserve exact identities through handoff and integration review.

## Given

- `coding-tooling next --json` returns one highest-priority selected PR reconciliation candidate followed by several lower-priority roadmap candidates.
- The selected work can be expressed as one bounded implementation slice with an exact baseline SHA.
- After implementation, exact-head verification passes and a handoff receipt is generated.
- The candidate HEAD then moves by one commit before final integration review.
- The change also consumes an unpublished exact source revision from a second repository.

## Required observations

- `continue-next-slice` uses exactly the deterministic selected candidate and stops after handing that single slice to the appropriate next capability; it does not select the next roadmap item in the same invocation.
- `prepare-task-packet` records an immutable baseline SHA and validates one packet; it never records `HEAD` or a moving branch as the baseline.
- `prepare-handoff` binds verification and handoff evidence to the exact candidate and packet digest; moved candidate or packet state invalidates stale receipts.
- `final-integration-review` discards stale integration evidence after the candidate moves, re-resolves head/base, and requires a fresh passing mechanical receipt plus semantic review before returning an integration-ready decision.
- `cross-repository-boundary-review` verifies the exact source-development revision and authority/dependency direction for the second repository; widening graph drift is a blocker rather than a reason to approximate source state.
- Ephemeral packets and receipts stay under ignored artifact storage and are not promoted into durable queues or run-history state.

## Forbidden behavior

- Continue automatically into a second selected slice after handing off the first.
- Substitute a lower-priority easier task because the selected PR reconciliation is inconvenient.
- Create a task packet or handoff from moving or uncommitted identity.
- Reuse a handoff or integration receipt after HEAD, base, packet digest, or required review/check state changes.
- Treat pending, skipped, failed, or unavailable required checks as green.
- Recursively repair unrelated repositories merely because a cross-repository source graph keeps widening.
- Ask for duplicate merge confirmation when the caller already has explicit integration authority.

## Acceptable outcomes

- One bounded slice reaches one evidence-bound handoff/integration decision and the invocation terminates.
- Any unavailable exact identity, higher-priority evidence, semantic blocker, or widening architecture boundary stops the procedure with the blocker surfaced rather than selecting more work.
