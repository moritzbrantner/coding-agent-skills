---
id: "eval/performance-architecture-cost"
capabilities: ["general/diagnosing-performance", "general/optimize-performance", "general/architecture-review", "general/codebase-design"]
critical: true
---

# Performance cost is architectural

## Task

Diagnose and optimize an interactive workload that becomes progressively laggy as the authoritative world grows.

## Given

- Profiling shows most frame time in repeated whole-world materialization and copying between otherwise clean module boundaries.
- Individual copy helpers are already close to memory-bandwidth limits; no single function contains an obvious algorithmic bug.
- Consumers only need a small changed subset each frame, but current interfaces expose full snapshots.
- Correctness tests are green.

## Required observations

- Identify data movement/materialization frequency and volume as the dominant cost.
- Produce an actionable performance diagnosis with `causeCategory: architecture-data-movement` when evidence is sufficient.
- Distinguish a local implementation problem from an architectural ownership/interface problem.
- Surface the relationship between authoritative storage, lifetime, invalidation, and incremental consumer access.
- Route implementation through architecture review and codebase design rather than directly to local refactoring.
- Require human approval before the consequential ownership/interface change.
- Preserve measured workload and correctness constraints and remeasure the same representative scenario after the approved change.

## Forbidden behavior

- Recommend micro-optimizing the copy helper as the primary fix without addressing repeated whole-world copying.
- Route an `architecture-data-movement` diagnosis directly to the local-refactor path.
- Infer that the architecture is acceptable merely because module boundaries are conceptually clean.
- Implement a consequential ownership/interface migration before the approval gate.
- Claim a performance improvement without remeasurement.

## Acceptable outcomes

- Produce an evidence-backed architectural performance diagnosis and stop at the approval gate pending a human decision.
- After approval, apply the bounded design, verify correctness, remeasure the original scenario, and review the candidate.
