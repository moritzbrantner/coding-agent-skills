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

Inspect the candidate diff, repository-local instructions, and the convention modules committed under `.conventions/`. Review against the policy actually installed for the candidate rather than fetching a newer central policy revision. Repository-local instructions remain the most specific policy and may override shared rules where they conflict.

When convention integrity matters, `coding-tooling conventions check` verifies the managed snapshots against `conventions.lock.json`. A central policy update is a separate explicit repository change, not part of reviewing an unrelated candidate. If `conventions.json` or `conventions.lock.json` shows that the repository has adopted installed policy but the snapshots are missing or corrupt, report that integrity failure and do not substitute either live policy or generic standards. Use `coding-tooling conventions resolve` only as a migration fallback for repositories that have not installed convention modules.

Report concrete findings against engineering standards: correctness risks, maintainability problems, test-policy violations, boundary mistakes, unsafe complexity, or convention violations. When a finding depends on shared policy, name the stable convention ID when practical. `conventions.lock.json` provides the installed policy revision when the caller records reproducibility metadata.

Keep findings evidence-based and independently understandable. Include the affected location/area, why it matters, and the smallest useful remediation direction. Do not modify code, do not collapse findings into a generic score, and do not invent requirements from a spec.

Only when the repository has not adopted installed conventions and the compatibility resolver is unavailable, state that limitation and use these minimal fallback standards: preserve correctness, respect existing repository patterns, keep interfaces small/cohesive, test public behavior, avoid unnecessary state/abstraction, and run repository-owned checks.
