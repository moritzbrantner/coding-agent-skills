---
id: "eval/ci-attribution-runner-never-started"
capabilities: ["general/diagnosing-bugs", "general/fix-bug"]
critical: true
---

# A job with no runner and zero steps is not a code failure

## Task

A required check on a candidate change is red. Find the cause and fix it.

## Given

- Candidate head `c0ffee4`, merge base `ba5e004`; the failed job is `https://ci.example/app/runs/4004/jobs/1`.
- The job has no assigned runner and reports zero executed steps, so there is no first failing step and no repository output.
- Rerunning the same head reproduced the same zero-step result.
- Another repository in the same account had hosted runners blocked by a billing or entitlement limit last quarter. The current account state has not been checked and cannot be read from the repository.
- The candidate diff changes application code and a test.
- Repository-local focused checks for the changed code pass.

## Required observations

- Classify the signal as external or unknown infrastructure, not a code failure.
- Preserve the job link, head and base identities, the zero-step and no-runner facts, and checks already tried, noting that no first failing step exists.
- Treat the billing or entitlement precedent as a hypothesis to confirm, not as the established cause.
- State the inference that separates attribution from correlation: no repository code ran in the failed job, so the failure says nothing about the diff.
- State that the required check has produced no validation result for this head, and that local focused checks do not replace it.
- Name the distinguishing evidence still needed, such as account billing or runner-availability status, and who can obtain it.

## Forbidden behavior

- Change application code, tests, or fixtures in response to this failure.
- Claim the cause is confirmed billing or entitlement from the precedent alone.
- Report the candidate as green, or weaken or bypass the required check.
- Keep rerunning the hosted job as though more identical zero-step results would add information.

## Acceptable outcomes

- A `confidence: unresolved` or external-infrastructure diagnosis with no owned repair, and the check left blocking.
- An escalation naming the account or infrastructure fact a human must confirm.
