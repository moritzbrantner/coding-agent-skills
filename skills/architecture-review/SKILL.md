---
id: "general/architecture-review"
name: "architecture-review"
description: "Review an existing codebase architecture for boundaries, ownership, coupling, and cost topology without changing code."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["architecture", "review", "design"]
requires: []
related-to: ["general/codebase-design", "general/improve-codebase-architecture", "general/diagnosing-performance"]
readiness: []
extensions:
  agent.procedure:
    schemaVersion: 1
    routing:
      useWhen: ["The task needs a read-only assessment of existing architecture, ownership, coupling, or cost topology."]
      doNotUseWhen: ["The task is to design a solution to an already-established architecture problem.", "An architecture design is already approved and the task is to implement it."]
      mutates: false
      approvalBoundary: "none"
    termination:
      terminal: true
      doneWhen: ["Material architecture findings are reported with evidence and consequence, or the review establishes that no material finding is supported."]
      stopWithoutChangeWhen: ["The inspected evidence does not support a material architecture finding."]
      escalateWhen: ["A consequential conclusion depends on unresolved product or domain intent.", "Repository guidance and implementation disagree and available evidence cannot establish the intended boundary."]
      evidenceRequired: ["Relevant module boundaries, dependencies, state ownership, repository guidance, and applicable cost topology were inspected."]
      outOfScope: ["Choosing a consequential target architecture without a design exercise.", "Implementing or migrating architecture changes."]
    artifacts:
      consumes: ["repository-state", "installed-policy", "architecture-context"]
      produces: ["architecture-findings"]
---

# Architecture Review

This is a read-only architecture review.

Inspect module/service boundaries, dependency direction, public surface area, state ownership, duplicated policy, cross-cutting coupling, and whether abstractions correspond to real independent concepts. Compare the code to settled ADR/domain knowledge, repository-local guidance, and the design doctrine in the installed `.conventions/` modules.

Also inspect the architecture's **cost topology**, especially on user-visible or otherwise hot paths. Trace important data from its authoritative owner to its consumers and examine:

- ownership and lifetime of large or frequently changing state;
- copy, clone, snapshot, materialization, and conversion boundaries;
- repeated transformations and derived representations;
- recomputation and invalidation scope when a small part of the source changes;
- allocation/churn and resource creation on hot paths;
- serialization/deserialization or marshaling between internal layers;
- process, thread, Wasm/host, CPU/GPU, device, network, or other expensive boundary crossings;
- synchronization points and ownership handoffs that serialize otherwise independent work;
- whole-world operations induced by interfaces when consumers need only a changed subset;
- abstractions that prevent incremental, borrowed/view-based, streamed, or otherwise direct access to authoritative data.

Do not assume an architecture is efficient because its modules are individually clean. A small interface can still force large hidden work on every call. Where runtime evidence exists, relate architectural paths to measured frequency and volume; where it does not, identify suspected cost mechanisms as hypotheses rather than performance findings.

Review against the policy committed to the repository rather than fetching a newer central policy revision. `coding-tooling conventions check` may verify the managed snapshots against their lock. If the repository has `conventions.json` or `conventions.lock.json` but the installed snapshots are missing or corrupt, report the installation failure instead of substituting live policy. Use `coding-tooling conventions resolve` only as a migration fallback for repositories that have not adopted installed convention modules.

Report specific findings with evidence and likely consequence. Distinguish among local implementation cleanup, algorithm/data-model changes, and genuinely architectural ownership/interface changes. Do not recommend a large migration merely because a different shape is aesthetically cleaner; consequential redesign needs evidence that the current boundary causes a material correctness, locality, evolvability, or runtime cost.

When docs and code disagree about intended architecture, investigate and surface the contradiction rather than declaring one side authoritative.

The output should be usable by `codebase-design`, `improve-codebase-architecture`, or a performance investigation. For cost findings, state the authoritative data involved, the expensive path or operation, its triggering frequency/scope when known, the consumer need, and why the current boundary causes unnecessary work. This skill does not mutate code. If the repository has not adopted shared conventions and the compatibility resolver is unavailable, state that limitation rather than silently assuming project-specific preferences.
