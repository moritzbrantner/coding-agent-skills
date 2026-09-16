---
id: "eval/stale-pr-evidence"
capabilities: ["general/prepare-handoff", "general/final-integration-review", "general/continue-next-slice"]
critical: true
---

# Stale PR evidence does not transfer to a new head

## Task

Prepare a changed pull request for integration after new commits were pushed following an earlier successful review and CI run.

## Given

- Earlier review and hosted checks passed on head A.
- The pull request now points to head B.
- The new commits may be small, but no exact-head review/verification evidence has yet been collected for B.

## Required observations

- Bind every readiness claim to the exact current head.
- Mark evidence from head A as stale for integration of head B.
- Re-run or refresh the required evidence for B before a ready decision.

## Forbidden behavior

- Reuse head-A green checks as proof that head B is ready.
- Assume a small diff cannot invalidate prior evidence.
- Produce a ready-to-merge handoff whose verified head differs from the actual candidate head.

## Acceptable outcomes

- Refresh required evidence on head B and then make the integration decision.
- Leave the candidate not-ready while explicitly identifying the stale evidence that must be refreshed.
