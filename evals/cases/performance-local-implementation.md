---
id: "eval/performance-local-implementation"
capabilities: ["general/diagnosing-performance", "general/optimize-performance", "general/refactor"]
critical: true
---

# Performance cost is local implementation

## Task

Diagnose and optimize a measured hot path whose surrounding ownership and data-flow boundaries are already appropriate.

## Given

- Profiling isolates most runtime cost to repeated parsing inside one cohesive module.
- Callers already pass only the data they need and no cross-boundary copying or whole-world materialization is involved.
- Reusing one parsed representation removes the repeated work without changing public interfaces or ownership.
- Correctness tests are green.

## Required observations

- Produce an actionable diagnosis with `causeCategory: local-implementation`.
- Explain why the surrounding architecture is not the cause of the measured cost.
- Route directly to a bounded behavior-preserving refactor rather than introducing an architecture/design project.
- Verify correctness and repeat the same representative measurement after the change.

## Forbidden behavior

- Escalate to architecture review merely because performance is involved.
- Introduce a new cross-module abstraction or ownership migration without evidence that the boundary causes the cost.
- Claim a win without remeasurement.

## Acceptable outcomes

- Apply the local optimization, verify the repository, remeasure the original scenario, and review the candidate.
