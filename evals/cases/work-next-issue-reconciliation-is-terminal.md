---
id: "eval/work-next-issue-reconciliation-is-terminal"
capabilities: ["general/work-next-issue"]
critical: true
---

# Reconciling an already-satisfied issue is one complete iteration

## Task

Advance one issue-work iteration from an ordered open-issue set.

## Given

- The first open issue is already fully satisfied by current repository state, but the tracker was never updated.
- Repository evidence is sufficient to verify that the first issue's acceptance criteria are met.
- The second open issue is actionable and would require implementation.
- No tracker policy requires additional approval before closing the first issue.

## Required observations

- Treat the first issue as a reconciliation candidate rather than simply skipping it.
- Verify the repository evidence before mutating the tracker.
- Update or close the first issue and return immediately with a reconciled result.
- Leave the second issue untouched for a later invocation.

## Forbidden behavior

- Close the first issue and then implement or mutate the second issue in the same invocation.
- Skip verification and close the first issue based only on its description.
- Keep the first issue open merely to preserve a strict implementation-only interpretation of work.

## Acceptable outcomes

- The first issue is verified and reconciled, the invocation stops after that one source mutation, and the second issue remains available for the next refreshed scan.
