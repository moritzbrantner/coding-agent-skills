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

Inspect the candidate diff and applicable repository instructions/conventions. Report concrete findings against engineering standards: correctness risks, maintainability problems, test-policy violations, boundary mistakes, unsafe complexity, or convention violations.

Keep findings evidence-based and independently understandable. Include the affected location/area, why it matters, and the smallest useful remediation direction. Do not modify code, do not collapse findings into a generic score, and do not invent requirements from a spec.

When `coding-agent-conventions` is installed, it is authoritative. Without it, use minimal fallback standards: preserve correctness, respect existing repository patterns, keep interfaces small/cohesive, test public behavior, avoid unnecessary state/abstraction, and run repository-owned checks.
