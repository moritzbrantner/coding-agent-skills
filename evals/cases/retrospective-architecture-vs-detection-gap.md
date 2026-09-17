---
id: "eval/retrospective-architecture-vs-detection-gap"
capabilities: ["general/engineering-retrospective"]
critical: true
---

# Architectural cause and late detection are different findings

## Task

Explain why a performance problem survived for several iterations and route prevention to the smallest owning layers.

## Given

- An interactive simulation became progressively laggy as authoritative world state grew.
- Profiling eventually showed repeated whole-world materialization and copying across clean module boundaries; consumers needed only changed subsets.
- The copy helpers themselves were already near memory-bandwidth limits.
- Earlier work repeatedly optimized local functions and added performance budgets without first tracing ownership, lifetime, copy frequency, and invalidation scope.
- Correctness tests stayed green throughout.
- Once cost topology was inspected, the architecture was changed to operate incrementally over authoritative state and performance improved materially under the same workload.

## Required observations

- Identify the direct technical cause as an architecture/data-movement problem, not a poorly implemented copy helper.
- Independently identify the detection gap: the development procedure and observability emphasized local hotspots/budgets before architecture-level cost topology and repeated-work evidence.
- Route the architectural repair/prevention to `architecture` and earlier cost-topology reasoning to `skill/procedure`; route missing repeatable copy/recompute evidence to `observability` only if the evidence actually shows it was unavailable.
- Distinguish prevention that could realistically have been applied earlier from hindsight-only knowledge.
- Recommend the smallest reusable prevention changes rather than a broad rewrite of every development procedure.

## Forbidden behavior

- Collapse direct cause and detection delay into one generic "performance issue".
- Recommend micro-optimizing the copy helper as the primary prevention.
- Claim that a performance budget alone would have prevented the architectural cost without evidence that it would identify the cause.
- Create a new shared convention merely because this incident was expensive.
- Implement the architecture or procedure change inside the retrospective.

## Acceptable outcomes

- Produce separate evidence-backed direct-cause and detection-gap findings with explicit owner-layer routing.
- Conclude that one or more proposed systemic changes are not justified when generalization evidence is missing.
- Stop after prevention routing; implementation is a separate slice.
