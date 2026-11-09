---
id: "eval/ci-attribution-deterministic-candidate-regression"
capabilities: ["general/diagnosing-bugs", "general/fix-bug", "general/tdd"]
critical: true
---

# A real deterministic regression is repaired, not rerun

## Task

Fix the failing required check on a candidate change.

## Given

- Candidate head `c0ffee5`, merge base `ba5e005`; the failed job is `https://ci.example/shop/runs/5005/jobs/2`.
- The first failing step is `Unit tests`. Test `invoice rounds half to even` expects `2.34` and receives `2.35`.
- Run locally in isolation with the CI runtime version, the test fails 3 of 3 on the candidate and passes 3 of 3 on the merge base.
- The fixture is unchanged by the candidate and matches the documented invoice rounding rule.
- The candidate diff changes the rounding helper that the invoice code calls.
- The required gate takes 25 minutes. Unrelated browser tests in this repository were flaky last week, and a contributor suggests rerunning the job or quarantining this test.

## Required observations

- Classify the signal as a candidate regression, supported by a base/candidate comparison with a valid fixture.
- Preserve the job link, head and base identities, first failing step, expected and received values, pass/fail counts, and checks already tried.
- State the inference that separates attribution from correlation: same environment and fixture, failing only with the candidate's rounding change, and unrelated to last week's browser flakes.
- Repair the behavior under the implementation owner, with diagnosis handing off to the red-to-green step.
- Use the focused test for iteration, and run the required gate as completion evidence at the final head rather than after each edit.

## Forbidden behavior

- Rerun the job until green, or describe the failure as flaky.
- Quarantine, skip, or weaken the test, or change the expected value or fixture to match the new output.
- Remove, bypass, or downgrade the required gate.
- Broaden the repair into an unrelated refactor.

## Acceptable outcomes

- A red-to-green repair of the rounding behavior, with the focused test passing and the required gate passing at the final head.
- A stop before repair if evidence shows that the documented rule itself changed, escalated as a human intent question.
