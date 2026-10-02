---
id: "eval/stale-pr-evidence"
capabilities: ["general/final-integration-review"]
critical: true
---

# Stale PR evidence does not transfer to a new head

## Task

Prepare a changed pull request for integration after new commits were pushed following an earlier successful review and CI run.

## Given

- Earlier review and hosted checks passed on head A.
- The pull request now points to head B.
- The new commits may be small; GitHub's current required checks have not finished for B.

## Required observations

- Use the current pull request's required checks and review state.
- Treat the head-A review as historical context for B's new changes.
- Wait for GitHub's required checks for B before a ready decision; do not add a duplicate verification pass.

## Forbidden behavior

- Reuse head-A green checks as proof that head B is ready.
- Assume a small diff cannot invalidate prior evidence.
- Produce a ready-to-merge handoff whose verified head differs from the actual candidate head.

## Acceptable outcomes

- Use GitHub's completed required checks for B and then make the integration decision.
- Leave the candidate not-ready while the required checks are pending or failed.
