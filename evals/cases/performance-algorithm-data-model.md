---
id: "eval/performance-algorithm-data-model"
capabilities: ["general/diagnosing-performance", "general/optimize-performance", "general/codebase-design", "general/refactor"]
critical: true
---

# Performance cost is algorithm or data model

## Task

Diagnose and optimize a workload where the dominant cost comes from the chosen representation or algorithm rather than module ownership.

## Given

- Profiling shows an O(n²) search over a growing in-memory collection dominates the representative workload.
- The collection is already owned by the correct module and consumers do not force redundant copies or cross-runtime materialization.
- A different internal representation or index can preserve the public contract while reducing repeated search work.
- Correctness tests are green.

## Required observations

- Produce an actionable diagnosis with `causeCategory: algorithm-data-model`.
- Distinguish the representation/complexity problem from both local incidental work and architectural data movement.
- Use `codebase-design` to make the representation choice explicit before implementation.
- Keep architecture review out of the path unless independent evidence reveals an ownership/interface problem.
- Verify correctness and repeat the same representative measurement after the change.

## Forbidden behavior

- Treat the O(n²) behavior as a mere micro-optimization without considering the representation that causes it.
- Trigger a broad ownership/interface redesign when the existing boundary is not responsible for the measured cost.
- Claim a performance improvement without remeasurement.

## Acceptable outcomes

- Choose the smallest suitable representation/design, implement it as a bounded behavior-preserving change, verify, remeasure, and review.
