---
id: "general/capability-internalization"
name: "capability-internalization"
description: "Evaluate whether an externally implemented capability should remain external, gain a stable boundary, or be replaced by a smaller specialized implementation using parity, performance, and consumer evidence."
kind: "skill"
maturity: "provisional"
entry-point: true
intents: ["dependency-replacement", "capability-internalization", "native-kernel", "performance"]
requires: []
related-to: ["general/diagnosing-performance", "general/optimize-performance", "general/prototype", "general/architecture-review", "general/codebase-design"]
readiness: []
extensions: {}
---

# Capability Internalization

Treat reimplementation as an evidence-backed experiment, not a cleanup goal.

Use this skill when an application depends on an external library, Docker/service boundary, subprocess, hosted API, FFI/WASM bridge, or other general-purpose implementation and there is a concrete reason to investigate whether a smaller or closer implementation would be better.

## Procedure

1. **Name the capability, not the product.** Record the actual behavior the consumer needs. Do not frame the task as "replace Redis", "replace Elasticsearch", or "rewrite library X" when the application consumes only a narrow subset.
2. **Establish the current implementation and workload.** Identify the real consumer, representative inputs, relevant scale, deployment constraints, and the existing implementation path including process, network, serialization, FFI, WASM, or other boundaries.
3. **Measure before proposing a replacement.** Use `general/diagnosing-performance` when performance is part of the motivation. Prefer `runtime-profiler` or repository-native measurement where available. Record other motivations such as portability, offline use, determinism, dependency surface, startup, embedding, or cross-project reuse explicitly.
4. **Decide whether a capability boundary is justified.** Introduce the smallest implementation-independent contract that expresses the consumed behavior when implementation variation is realistic or an experiment requires side-by-side implementations. Do not add speculative abstraction around every dependency.
5. **Keep the existing implementation as the reference.** Wrap it behind the same contract when practical. Preserve its behavior as an oracle or fixture producer for comparison.
6. **Build the smallest candidate.** Implement only the capability subset required by the target workload. Prefer a focused native kernel when it provides a concrete systems, performance, portability, or reuse advantage. Do not recreate unrelated upstream features.
7. **Prove behavioral compatibility.** Use differential tests, property tests, fuzzing, fixtures, protocol conformance, or a specification as appropriate. Compare candidate and reference on edge cases as well as representative data. State intentional semantic differences explicitly instead of hiding them behind tolerance.
8. **Measure the candidate on the same workload.** Compare the metrics that matter for the reason the experiment exists: latency, throughput, CPU, memory, allocations, startup, package/binary size, I/O, serialization, IPC/network cost, or another declared metric. Use Moonlight or another evaluator when compatible evidence is available; do not infer a win from code shape.
9. **Validate one real consumer.** A reusable kernel is not justified solely by a benchmark. Exercise the candidate through at least one actual application or package boundary and confirm that the consumed surface is sufficient.
10. **Make an explicit decision.** Choose one of: `keep-external`, `keep-both`, `native-candidate-needs-more-evidence`, `make-native-default`, or `remove-external`. A result of `keep-external` is a successful experiment when evidence shows the external implementation remains the better trade-off.
11. **Extract only proven reuse.** If the candidate is useful outside its first consumer, move the stable core into the narrowest reusable foundation. Avoid a monolithic utilities package and avoid exposing implementation details that the consumer contract does not need.
12. **Remove temporary complexity.** After the decision stabilizes, remove shadow execution, unused adapters, benchmarks that no longer represent a decision boundary, and other experiment-only scaffolding unless they remain valuable regression evidence.

## Candidate record

Use a compact durable record in an issue, spec, ADR, or repository-owned experiment artifact:

```yaml
capability: full-text-search
consumer: <real consumer>
current:
  implementation: <library/service/process>
  boundary: <in-process|ffi|wasm|subprocess|network|hosted>
consumed:
  - <capability>
motivation:
  - <measurable or architectural reason>
workload: <representative workload/fixture/scenario>
reference: <reference implementation or specification>
candidate: <implementation or planned kernel>
evidence:
  correctness: []
  performance: []
  consumer: []
decision: <keep-external|keep-both|native-candidate-needs-more-evidence|make-native-default|remove-external>
```

Do not fill unknown evidence with guesses. An `external-for-now` or `keep-external` decision is preferable to manufacturing a replacement rationale.

## Guardrails

Raise the evidence bar sharply for cryptography, TLS, database engines, distributed consensus, durability-critical storage, and similarly security- or reliability-sensitive infrastructure. Prefer internalizing a narrow surrounding capability over recreating those systems wholesale.

Do not equate generated-code cost with maintenance cost. Correctness surface, semantic compatibility, operations, security, long-term ownership, and failure modes remain part of the decision even when implementation is cheap.

Follow the repository's installed engineering policy. In repositories adopting `PRINCIPLE-007`, external implementations are valid bootstrap choices and replacement is optional unless evidence establishes a concrete reason to proceed.
