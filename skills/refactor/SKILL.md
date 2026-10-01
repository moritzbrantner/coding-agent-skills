---
id: "general/refactor"
name: "refactor"
description: "Perform behavior-preserving structural improvement from a green baseline."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["refactor", "cleanup", "structure"]
requires: []
related-to: ["general/tdd", "general/codebase-design", "general/implement"]
readiness: []
extensions: {}
---

# Refactor

Refactor any behavior-preserving structural improvement; generated/prototype code is only a common use case, not a separate ownership domain.

## Preconditions and boundaries

- Start from green relevant tests/checks. If there is no trustworthy green baseline, stop and establish one through the appropriate caller.
- Preserve externally intended behavior. Feature work and behavior correction are out of scope.
- Inspect after implementation even when the likely answer is `no-refactor-needed`; the implementing agent should not be the sole judge of its own generated structure.
- Routine/local cleanup, including cohesive private file decomposition that preserves public interfaces, ownership, dependencies, and behavior, may proceed automatically from the green baseline.
- A substantial authority, public API, persistence/protocol, ownership/lifecycle/dependency boundary, or hard-to-reverse architecture change needs explicit owner intent before mutation. Reuse an already settled decision; when intent remains unresolved, ask one focused consequential question.
- If you discover tests that are insensitive, misleading, or wrong enough to permit a major oversight, stop and explain the issue to the human. Do not silently rewrite those tests under the banner of refactoring.
- A behavior correction returns through TDD once the human has decided the intended behavior.

Work in small, reviewable, behavior-preserving slices and re-run focused verification after each meaningful change.

Apply the resolved DESIGN-* rules by reference. A private source file is not automatically a new public module boundary. For an architectural extraction, establish what is difficult today, the real boundary introduced, what becomes easier to change, and the additional indirection. A line count alone does not justify extraction or refusal to organize cohesive private files. Report `no-refactor-needed` when no useful improvement is supported, while still surfacing real boundary defects.

Reuse the task’s [resolved policy context](../../docs/policy-context.md) for applicable engineering doctrine and repository-local exceptions. Stack-specific style, extraction criteria, and module vocabulary come from that context; cache-integrity checks do not replace behavior verification.
