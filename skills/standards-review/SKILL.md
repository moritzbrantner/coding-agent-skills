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

Inspect the candidate diff and reuse the task’s [resolved policy context](../../docs/policy-context.md). Apply repository-local exceptions through the shared precedence rules.

Cache integrity and repair belong to the normal tooling entry path described in the policy-context procedure, not an independent review-specific authority decision.

Report concrete findings against engineering standards: correctness risks, maintainability problems, test-policy violations, boundary mistakes, unsafe complexity, or convention violations. When a finding depends on shared policy, name the stable convention ID when practical. Use the resolved `sourceRevision` when the caller records reproducibility metadata.

Treat heuristic detector output as evidence rather than policy. A `coding-tooling findings` signal or score becomes blocking only when repository policy explicitly promotes that detector or independent review evidence establishes a concrete standards defect. Keep unpromoted detector output clearly advisory.

Keep findings evidence-based and independently understandable. Include the affected location/area, why it matters, and the smallest useful remediation direction. Do not modify code, do not collapse findings into a generic score, and do not invent requirements from a spec.

When policy evidence is unavailable, report the limitation. Continue reviewing observable correctness and repository-owned requirements without claiming shared-policy conformance.
