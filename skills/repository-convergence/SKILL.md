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

Establish the convergence workset before selecting a repair. Count only material, currently actionable convergence findings in the selected scope; do not count cosmetic advice, speculative opportunities, ordinary roadmap issues, or healthy open pull requests merely to make the number larger. Prefer stable finding identities and the `initialFindingIds`, `rounds`, and `finalFindingIds` evidence from `coding-tooling converge` when available. Record the initial count before mutation so the slice can prove whether the remaining work actually shrank.

Choose the strongest reproducible repository-owned finding that materially improves correctness, boundary integrity, validation fidelity, dependency/release behavior, or deployment/runtime parity. Prefer a narrow vertical repair over broad cleanup. Do not manufacture a substitute baseline merely because a stronger candidate finding is inconvenient to reproduce.

Make the smallest coherent change that resolves the selected finding. Keep ownership at the proper boundary: fix upstream defects upstream, preserve source-first cross-repository dependency validation, and do not duplicate deterministic mechanics that belong in `coding-tooling` or shared doctrine that belongs in `coding-agent-conventions`.

Validate progressively. Run the narrowest affected check first, then the repository's canonical deterministic verification/conformance, then exact-head hosted CI when available. If a broader check exposes a regression caused by the repair, fix and revalidate the smallest affected scope before broadening again.

After validation, re-observe the same convergence scope exactly once. Report a convergence trajectory such as `5 -> 4`, together with the stable findings resolved and any material findings introduced. The target is monotonic non-increasing work with a net decrease for a successful repair slice. Resolving one finding while introducing one material finding is `5 -> 5`, not progress. If the repair caused the introduced finding, repair that regression within the same slice before claiming convergence. If an unrelated new finding appears, report it and stop rather than selecting a second independent repair.

Do not use commit count, pull-request count, or total GitHub issue count as a substitute for convergence progress. Those inventories can stay flat or grow while the material convergence workset shrinks.

Return the baseline, selected finding, repair performed, validation evidence, exact verified head when available, convergence trajectory, resolved and introduced findings, net change in the material workset, and any residual advisory findings or next convergence seam. Explicitly label the slice as decreasing, flat, regressed, or already at zero. Stop after this slice; another invocation may choose the next independent finding.
