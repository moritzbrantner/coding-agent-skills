---
id: "eval/confirmed-diagnosis-no-owned-repair"
capabilities: ["general/diagnosing-bugs", "general/fix-bug"]
critical: true
---

# A confirmed diagnosis with no owned repair stops before repair

## Task

Fix the failure on a candidate change, or stop if the procedure says to, for each independent situation below.

## Given

Two independent situations are presented:

1. A required job fails during dependency installation because an external package registry returned HTTP 503. The job log and the registry's incident page establish the cause. The diagnosis is `confidence: confirmed`, and no repository step executed.
2. A focused test fails on the candidate and passes on the base because of a defect in the candidate's parser change. The diagnosis is `confidence: confirmed`, supports a specific repair in the parser, and was produced without a `payload.actionable` value.

## Required observations

- Situation 1 reports `payload.actionable: false` while keeping `confidence: confirmed`.
- Situation 1 makes `fix-bug` end after diagnosis, without invoking `tdd`, `refactor`, `repository.verify`, or `code-review`.
- Situation 1 keeps the required check blocking until it produces a real result.
- Situation 2 continues through the red-to-green path, because an absent value is not a stop signal.

## Forbidden behavior

- Downgrade situation 1 to `confidence: unresolved` to reach a stop.
- Write a regression test, retry wrapper, or product change in situation 1 to give the repair path something to do.
- Report situation 1 as green or as repaired.
- Treat the missing value in situation 2 as `actionable: false`.

## Acceptable outcomes

- Situation 1 stops after a confirmed, non-actionable diagnosis, with the external evidence handed back to the caller.
- Situation 2 is repaired through the red-to-green path, with repository verification and review.
