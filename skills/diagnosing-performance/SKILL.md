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
extensions: {}
---

# Diagnosing Performance

Measure before optimizing.

Define the relevant user-visible or system-visible metric and a representative workload. Establish a reproducible baseline, profile the dominant cost, separate CPU/memory/I/O/latency/throughput effects where relevant, and test hypotheses against measurements.

Do not stop at the hottest function. Trace the measured cost through the data and execution path that causes it. Inspect the **cost topology** when relevant: authoritative data ownership and lifetime, copy/materialization boundaries, repeated transformations, recomputation and invalidation scope, allocation/churn, serialization or marshaling, synchronization, runtime/device crossings, and whole-world work performed for consumers that need only a subset.

Use the existing `suspected cause` portion of the shared performance diagnosis envelope to make the level of the dominant cause explicit when evidence supports it:

- **local implementation** — avoidable work inside an otherwise appropriate boundary;
- **algorithm/data model** — complexity or representation causes the cost even with appropriate ownership;
- **architecture/data movement** — ownership, interfaces, lifetimes, synchronization, or boundary crossings cause repeated work that local optimization cannot remove;
- **external/environment** — the dominant cost is outside the owned implementation or depends on the execution environment;
- **unresolved** — evidence is not yet sufficient to classify the cause.

This classification is diagnostic evidence, not a new interchange schema. Do not invent fields outside the current `agent-contracts` performance diagnosis envelope.

A locally expensive helper may be only the symptom of an architectural decision—for example, a fast bulk copy that is needlessly repeated every frame. Conversely, do not label a problem architectural merely because a boundary appears in the profile: establish that changing the boundary or ownership could remove material repeated work that the current contract requires.

Produce the shared performance diagnosis envelope from `agent-contracts`: workload, baseline, bottleneck evidence, confidence, suspected cause, measurement method, and constraints that must not regress. Preserve enough detail to repeat the same workload after a later change.

Do not optimize code in this skill. Instrumentation or benchmark scaffolding may be created as evidence when necessary, but keep it separable from the eventual implementation. When the dominant cause is architectural/data movement, make that handoff explicit so `architecture-review`/`codebase-design` can reason about the consequential ownership or interface change instead of treating the problem as a local refactor.

Prefer `runtime-profiler` or repository-native profilers when available, but remain usable with ordinary platform tooling. Never infer a performance win from code shape alone, and never claim that a cleaner architecture is faster without measurement.
