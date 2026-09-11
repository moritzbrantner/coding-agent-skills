---
id: "general/repository-convergence"
name: "repository-convergence"
description: "Converge a repository toward its installed policy and deterministic tooling using one strongest real finding, a narrow repair, and repository-owned evidence."
kind: "skill"
maturity: "provisional"
entry-point: true
intents: ["converge", "repository", "maintenance", "quality"]
requires: []
related-to: ["general/standards-review", "general/code-review", "general/review-and-fix"]
readiness: []
extensions: {}
---

# Repository Convergence

Use this skill for one bounded repository-convergence slice. It does not create durable queues, schedule future work, or replace repository-owned verification.

Read the repository's `AGENTS.md`, installed `.conventions/index.md` and only the relevant convention modules, `.coding-tooling.json`, repository-owned validation commands, and current hosted-CI state when available. If the repository declares installed conventions but the managed snapshot is missing or corrupt, report that integrity failure instead of substituting live policy.

Establish a real baseline before editing: identify the exact branch/head under review, run or inspect the repository-owned deterministic checks, and distinguish blocking failures from advisory evidence. `coding-tooling findings` and similar heuristic detectors are evidence, not policy: do not turn a score or heuristic signal into a blocker unless repository policy explicitly promotes it or independent evidence establishes a defect.

Choose the strongest reproducible repository-owned finding that materially improves correctness, boundary integrity, validation fidelity, dependency/release behavior, or deployment/runtime parity. Prefer a narrow vertical repair over broad cleanup. Do not manufacture a substitute baseline merely because a stronger candidate finding is inconvenient to reproduce.

Make the smallest coherent change that resolves the selected finding. Keep ownership at the proper boundary: fix upstream defects upstream, preserve source-first cross-repository dependency validation, and do not duplicate deterministic mechanics that belong in `coding-tooling` or shared doctrine that belongs in `coding-agent-conventions`.

Validate progressively. Run the narrowest affected check first, then the repository's canonical deterministic verification/conformance, then exact-head hosted CI when available. If a broader check exposes a regression caused by the repair, fix and revalidate the smallest affected scope before broadening again.

Return the baseline, selected finding, repair performed, validation evidence, exact verified head when available, and any residual advisory findings or next convergence seam. Stop after this slice; another invocation may choose the next finding.
