---
id: "general/tdd"
name: "tdd"
description: "Implement an approved behavior slice with a strict red-to-green test-driven loop."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["implement", "test", "behavior-change"]
requires: []
related-to: ["general/implement", "general/refactor", "general/fix-bug"]
readiness: []
extensions: {}
---

# TDD

TDD is strictly **red -> green**. Refactoring is deliberately outside this skill.

Consume the approved testing strategy from the caller, spec, or ticket. Do not interrupt the user to reconfirm an already settled seam. For a direct invocation, infer established public seams from repository-local guidance, installed conventions, and repository structure; ask only when there is a genuine unresolved design choice.

For one bounded behavior slice:

1. Add or select a test through a public/meaningful seam.
2. Run it and confirm it fails for the intended reason.
3. Make the smallest implementation change that satisfies the behavior.
4. Run the focused test until green, then the relevant affected checks.
5. Stop at green and hand back to the caller.

Do not perform cleanup/refactoring here. Do not change behavior merely to make a test convenient. Avoid mocks when a stable public seam can exercise the behavior; use test doubles only at real boundaries.

Read the repository's installed `.conventions/` testing policy when present, subject to repository-local overrides. `coding-tooling conventions check` can verify that the installed snapshots match their lock. If the repository has adopted installed policy and that installation is broken, report the failure rather than substituting live policy. Use `coding-tooling conventions resolve` only as a migration fallback for repositories that have not adopted installed convention modules.
