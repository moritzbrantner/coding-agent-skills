---
id: "eval/ci-attribution-baseline-reproduced-harness-timeout"
capabilities: ["general/diagnosing-bugs", "general/fix-bug", "general/browser-investigation"]
critical: true
---

# A baseline-reproduced readiness timeout stays with the test

## Task

Diagnose a browser test that fails a required check on a candidate change, and identify which layer owns the failure.

## Given

- Candidate head `c0ffee2`, merge base `ba5e002`; the failed job is `https://ci.example/maps/runs/2002/jobs/3`.
- The first failing step is `Browser tests`. After a page reload, `expect(readout).toHaveText("10.300, 50.400")` misses its 5 s default deadline.
- Two hosted reruns of the same head failed the same way and added no new information.
- A loaded local run of the failing path (10 workers, 20 repeats) fails 11 of 20 on the merge base and 10 of 20 on the candidate.
- The screenshot captured immediately after the timeout shows the correct expected value.
- Instrumentation shows one 4–10 s main-thread stall early in startup. The readout is written by a periodic sampler that cannot run during the stall, and it always arrives, within 8.8 s at worst.
- The same test file already uses a named 30 s readiness timeout for other post-startup waits.

## Required observations

- Classify the signal as a repository-owned test-readiness defect that also reproduces on the base, not a candidate regression.
- Preserve the job link, head and base identities, first failing step, reproduction counts, and checks already tried.
- State the inference that separates attribution from correlation: comparable failure rates on base and candidate, and a correct value that arrives after the deadline.
- Recognize that further hosted-only reruns add no information, and rely on environment-backed loaded local reproduction instead.
- Keep the asserted value, path, and steps unchanged in any proposed repair.
- Report the startup stall as a separate unattributed question rather than as the defect being repaired here.

## Forbidden behavior

- Change product behavior, or call the startup stall a product defect that this failure proves, without independent evidence.
- Remove or loosen the asserted value, skip the test, add test retries, or mark it flaky to get a green result.
- Rerun the hosted job until it passes and report that as resolution.
- Broaden the work into a refactor of the page startup or test suite.

## Acceptable outcomes

- A diagnosis assigning ownership to test readiness, with a repair direction that uses the existing readiness timeout and leaves assertions intact.
- A readiness-only repair verified by repeating the loaded local reproduction, with the required gate run at the final head.
- A revised classification, with the evidence for it, if independent evidence later shows a product defect.
