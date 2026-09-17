---
id: "eval/retrospective-agent-cost-evidence"
capabilities: ["general/engineering-retrospective"]
critical: true
---

# Agent cost evidence informs prevention without becoming a productivity score

## Task

Use correlated agent-run evidence to explain why a repair converged slowly and route the smallest reusable prevention improvement.

## Given

- Three comparable attempts share the same task/workload identity and exact baseline revision.
- Attempts 1 and 2 repeatedly optimized local helpers, each producing candidates that passed correctness checks but did not materially improve the measured runtime problem.
- Their evidence shows high token use, repeated CI waits, and multiple repair invocations before the performance diagnosis was classified.
- Attempt 3 ran cost-topology diagnosis first, classified the dominant cause as `architecture-data-movement`, changed the ownership/dataflow boundary after approval, and materially improved the same scenario with fewer subsequent retries.
- A fourth unrelated task used a different workload, environment, and provider and happened to use fewer tokens overall.

## Required observations

- Correlate attempts using producer-owned run/attempt identity plus exact workload/source provenance, not filenames or timestamps.
- Use token, timing, retry, CI/wait, and escalation measurements as explanatory evidence for the late-diagnosis pattern, not as a single performance or productivity score.
- Do not compare the unrelated fourth task as evidence that its provider/model is better or more efficient.
- Separate the direct technical cause (`architecture-data-movement`) from the detection gap (local optimization was attempted before cost-topology diagnosis).
- Route earlier architecture/data-movement diagnosis to `skill/procedure`; keep portable measurement semantics in `performance-evidence` and run/attempt identity ownership in the orchestrator rather than moving either into the skill.
- Require recurrence/comparability evidence before proposing a shared convention or deterministic policy.

## Forbidden behavior

- Rank people, providers, or models from token count or elapsed time across incomparable tasks.
- Treat missing telemetry as zero.
- Put Performance Evidence interpretation into reusable-workflows merely because that layer transported the artifact.
- Recommend a landscape-wide policy solely because one attempt was expensive.
- Collapse the architecture cause and procedure detection gap into one finding.

## Acceptable outcomes

- Produce an evidence-backed retrospective that identifies the expensive late-routing pattern and recommends the smallest skill/procedure improvement.
- Keep provider/model information as context only where it materially affects comparable execution semantics.
- Conclude that stronger shared policy is not yet justified if the available examples do not generalize beyond this workload class.
