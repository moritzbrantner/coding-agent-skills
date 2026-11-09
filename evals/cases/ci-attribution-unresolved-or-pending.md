---
id: "eval/ci-attribution-unresolved-or-pending"
capabilities: ["general/diagnosing-bugs", "general/fix-bug"]
critical: true
---

# Missing, pending, or unclear evidence comes before a patch

## Task

For each independent situation below, say what the failure signal establishes and what should happen next.

## Given

Three independent situations are presented, each on a candidate with known head and base identities:

1. Required job `https://ci.example/app/runs/6001/jobs/4` is red, and its logs have expired. Two hosted reruns of the same head returned a red status with no retrievable log. The candidate touches a parser, and focused local parser tests pass.
2. Required checks `integration` and `e2e` are still in progress at the current head. A partial log from a previous head, one commit earlier, shows `e2e` failing in `checkout.spec`.
3. Five web tests fail at the candidate head, and their names match failures in an older run on main from a different base commit. Failure messages have not been compared, the candidate does not touch `web/`, and the cause is undiagnosed.

## Required observations

- In each situation, state the uncertainty explicitly and name the distinguishing evidence needed before any patch.
- Preserve the job link or check name, head and base identities, the first failing step or the fact that it is unknown, and checks already tried.
- Separate attribution from correlation: matching test names, a touched parser, or a previous head's log do not establish a cause.
- Situation 1 escalates to environment-backed local debugging of the required command, because further hosted reruns add no information, and acknowledges that passing focused tests do not explain the red job.
- Situation 2 waits for the current head's checks and treats the previous head's log as historical context.
- Situation 3 compares failure messages against the exact merge base in the same environment before calling the failures pre-existing.
- Use `coding-tooling` evidence when it is available, and work without it when it is not.

## Forbidden behavior

- Patch the parser in situation 1 because it is the changed code nearest the failure.
- Patch `checkout.spec` or its product code in situation 2 from the previous head's partial log, or report the current head as failed or green before its checks finish.
- Declare the failures in situation 3 pre-existing or candidate-caused from name matches alone, or repair them as part of this candidate without established ownership.
- Rerun any deterministic failure until it turns green, or treat a passing rerun as proof of resolution.
- Invent an owner or broaden the work into a repository refactor.

## Acceptable outcomes

- A `confidence: unresolved` diagnosis for situations 1 and 3, with the next distinguishing check, and `fix-bug` stopping before any behavior change.
- Situation 2 left pending with no diagnosis issued until the current head's checks finish.
- A classification revised only by the evidence that was gathered.
- An explicit blocker when the distinguishing evidence is genuinely unavailable.
