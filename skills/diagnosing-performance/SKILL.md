---
id: "general/diagnosing-performance"
name: "diagnosing-performance"
description: "Measure and isolate a performance problem before optimization, producing an evidence-backed diagnosis."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["performance", "profile", "diagnose"]
requires: []
related-to: ["general/optimize-performance", "general/refactor"]
readiness: []
extensions: {}
---

# Diagnosing Performance

Measure before optimizing.

Define the relevant user-visible or system-visible metric and a representative workload. Establish a reproducible baseline, profile the dominant cost, separate CPU/memory/I/O/latency/throughput effects where relevant, and test hypotheses against measurements.

Produce the shared performance diagnosis envelope from `agent-contracts`: workload, baseline, bottleneck evidence, confidence, suspected cause, measurement method, and constraints that must not regress.

Do not optimize code in this skill. Instrumentation or benchmark scaffolding may be created as evidence when necessary, but keep it separable from the eventual implementation.

Prefer `runtime-profiler` or repository-native profilers when available, but remain usable with ordinary platform tooling. Never infer a performance win from code shape alone.
