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
- Routine/local cleanup may proceed automatically.
- A substantial module-boundary, public-interface, or architectural change requires human approval **before** the consequential change.
- If you discover tests that are insensitive, misleading, or wrong enough to permit a major oversight, stop and explain the issue to the human. Do not silently rewrite those tests under the banner of refactoring.
- A behavior correction returns through TDD once the human has decided the intended behavior.

Work in small, reviewable, behavior-preserving slices and re-run focused verification after each meaningful change.

Stack-specific style, extraction thresholds, module vocabulary, and design doctrine come from repository-local guidance plus the installed convention modules. Read `AGENTS.md` and the relevant entries under `.conventions/` when present. `coding-tooling conventions check` may verify that the managed snapshots are intact, but it is not a substitute for tests or repository verification. If the repository has adopted installed policy and that installation is broken, report the failure rather than substituting live policy. Use `coding-tooling conventions resolve` only as a migration fallback for repositories that have not installed convention modules.
