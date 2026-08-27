---
id: "general/architecture-review"
name: "architecture-review"
description: "Review an existing codebase architecture and report boundary, coupling, and ownership findings without changing code."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["architecture", "review", "design"]
requires: []
related-to: ["general/codebase-design", "general/improve-codebase-architecture"]
readiness: []
extensions: {}
---

# Architecture Review

This is a read-only architecture review.

Inspect module/service boundaries, dependency direction, public surface area, state ownership, duplicated policy, cross-cutting coupling, and whether abstractions correspond to real independent concepts. Compare the code to settled ADR/domain knowledge and the design doctrine in the current resolved `coding-agent-conventions` stack.

When shared conventions are available, obtain the applicable stack through `coding-tooling conventions resolve`, read the returned files, and apply repository-local instructions as the most specific policy. Do not rely on consumer-side copies of shared doctrine.

Report specific findings with evidence and likely consequence. Distinguish local cleanup from a genuinely consequential redesign. Do not recommend a large migration merely because a different shape is aesthetically cleaner.

When docs and code disagree about intended architecture, investigate and surface the contradiction rather than declaring one side authoritative.

The output should be usable by `codebase-design` or `improve-codebase-architecture`; this skill does not mutate code. If live convention resolution is unavailable, state that limitation rather than silently assuming project-specific preferences.