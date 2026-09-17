---
id: "general/diagnosing-performance"
name: "diagnosing-performance"
description: "Measure and isolate a performance problem before optimization, including whether the dominant cost is local, algorithmic, or architectural."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["performance", "profile", "diagnose"]
requires: []
related-to: ["general/optimize-performance", "general/refactor", "general/architecture-review", "general/codebase-design"]
readiness: []
extensions:
  agent.procedure:
    schemaVersion: 1
    routing:
      useWhen: ["A performance symptom needs a reproducible workload, measured baseline, bottleneck evidence, and dominant-cause classification before optimization."]
      doNotUseWhen: ["A trusted performance diagnosis already provides an actionable owned-code cause and the task is to implement and remeasure the optimization.", "The primary problem is incorrect behavior rather than measured performance."]
      mutates: true
      approvalBoundary: "none"
    termination:
      terminal: true
      doneWhen: ["The performance diagnosis records the representative scenario, measured baseline, bottlenecks, constraints, candidate directions, actionability, dominant cause category, and enough method detail to repeat the measurement."]
      stopWithoutChangeWhen: ["Existing profiling evidence is sufficient and no instrumentation or benchmark scaffold needs to be added.", "The dominant cause is external/environmental or remains unresolved, so no owned code optimization is justified by the current evidence."]
      escalateWhen: ["The representative workload, success metric, or acceptable tradeoff depends on unresolved human intent."]
      evidenceRequired: ["A reproducible scenario, measured baseline, dominant bottleneck evidence, tested cause hypotheses, and explicit uncertainty are preserved."]
      outOfScope: ["Implementing the optimization.", "Claiming a speedup from code or architecture shape without remeasurement."]
    artifacts:
      consumes: ["request-context", "repository-state", "runtime-evidence"]
      produces: ["performance-diagnosis", "performance-evidence"]
---

# Diagnosing Performance

Measure before optimizing.

Define the relevant user-visible or system-visible metric and a representative workload. Establish a reproducible baseline, profile the dominant cost, separate CPU/memory/I/O/latency/throughput effects where relevant, and test hypotheses against measurements.

Do not stop at the hottest function. Trace the measured cost through the data and execution path that causes it. Inspect the **cost topology** when relevant: authoritative data ownership and lifetime, copy/materialization boundaries, repeated transformations, recomputation and invalidation scope, allocation/churn, serialization or marshaling, synchronization, runtime/device crossings, and whole-world work performed for consumers that need only a subset.

Produce the shared `agent.diagnosis-envelope/v1` performance payload from `agent-contracts`: scenario, measured baseline, bottlenecks, constraints, and candidate optimization directions. When the contract revision supports performance-routing metadata, always populate:

- `payload.actionable` — `true` only when the evidence supports a specific owned code-optimization path without first revising the workload or constraints;
- `payload.causeCategory` — exactly one of `local-implementation`, `algorithm-data-model`, `architecture-data-movement`, `external-environment`, or `unresolved`.

Classify the dominant cause as follows:

- **`local-implementation`** — avoidable work inside an otherwise appropriate boundary;
- **`algorithm-data-model`** — algorithmic complexity or representation causes the cost even with appropriate ownership;
- **`architecture-data-movement`** — ownership, interfaces, lifetimes, synchronization, or boundary crossings require repeated work that local optimization cannot remove;
- **`external-environment`** — the dominant measured cost is outside the owned implementation or belongs to an operational/environment boundary rather than a code optimization in the diagnosed component;
- **`unresolved`** — evidence is not yet sufficient to select a cause level.

Use `actionable: false` for `unresolved`. Use `actionable: false` for `external-environment` when the remedy belongs outside the diagnosed code component; report the external evidence and hand the decision back to the caller instead of disguising an operational change as a code optimization. For the three owned code categories, mark the diagnosis actionable only when the evidence and candidate optimization direction are specific enough to implement and remeasure.

A locally expensive helper may be only the symptom of an architectural decision—for example, a fast bulk copy that is needlessly repeated every frame. Conversely, do not label a problem architectural merely because a boundary appears in the profile: establish that changing the boundary or ownership could remove material repeated work that the current contract requires.

Preserve enough measurement detail to repeat the same scenario after a later change. Do not optimize code in this skill. Instrumentation or benchmark scaffolding may be created as evidence when necessary, but keep it separable from the eventual implementation. When the dominant cause is `architecture-data-movement`, make the ownership/interface evidence explicit so `architecture-review` and `codebase-design` can reason about the consequential change instead of treating the problem as a local refactor.

Prefer `runtime-profiler` or repository-native profilers when available, but remain usable with ordinary platform tooling. Never infer a performance win from code shape alone, and never claim that a cleaner architecture is faster without measurement.
