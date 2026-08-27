---
id: "general/standards-review"
name: "standards-review"
description: "Review a candidate against repository and coding-agent engineering standards without modifying it."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["review", "standards", "quality"]
requires: []
related-to: ["general/code-review", "general/review-and-fix"]
readiness: []
extensions: {}
---

# Standards Review

This is a read-only review axis.

Inspect the candidate diff and applicable repository instructions/conventions. When `coding-agent-conventions` is available, resolve the current stack with `coding-tooling conventions resolve` for the repository under review and read the returned convention files. Do not rely on a copied convention snapshot in the consumer repository. Repository-local instructions remain the most specific policy and may override shared rules where they conflict.

Report concrete findings against engineering standards: correctness risks, maintainability problems, test-policy violations, boundary mistakes, unsafe complexity, or convention violations. When a finding depends on shared policy, name the stable convention ID when practical. Keep the resolver's `sourceRevision` with review evidence when the caller records reproducibility metadata.

Keep findings evidence-based and independently understandable. Include the affected location/area, why it matters, and the smallest useful remediation direction. Do not modify code, do not collapse findings into a generic score, and do not invent requirements from a spec.

If live convention resolution is unavailable, state that limitation and use only these minimal fallback standards: preserve correctness, respect existing repository patterns, keep interfaces small/cohesive, test public behavior, avoid unnecessary state/abstraction, and run repository-owned checks.