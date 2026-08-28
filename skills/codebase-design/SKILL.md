---
id: "general/codebase-design"
name: "codebase-design"
description: "Apply stable design doctrine to a concrete codebase architecture problem and compare viable structures."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["design", "architecture", "modules"]
requires: []
related-to: ["general/architecture-review", "general/improve-codebase-architecture", "general/domain-modeling"]
readiness: []
extensions: {}
---

# Codebase Design

Apply the stable design vocabulary from repository-local guidance and the installed `.conventions/` modules to the concrete problem; do not duplicate that doctrine here. The policy committed in the consumer repository is the policy context for the design exercise.

If `conventions.json` or `conventions.lock.json` shows that the repository has adopted installed policy but `.conventions/` is missing or corrupt, report the broken installation and stop relying on shared policy until it is repaired. Do not substitute a live, potentially newer policy revision. Use `coding-tooling conventions resolve` only as a migration fallback for repositories that have not adopted installed convention modules.

Understand the behavior and ownership boundaries first. Identify where state, policy, data, and side effects naturally belong. Prefer cohesive deep modules, small public surfaces, locality, and progressive composition when those principles are part of the applicable installed policy.

Consider at least two plausible designs when the choice is consequential. Compare them against coupling, reversibility, operational complexity, testability, migration cost, and how much new abstraction they require. Recommend the smallest design that solves the actual boundary problem.

This skill designs; it does not silently perform a broad architectural migration. Settled consequential choices can be handed to domain/ADR documentation and an implementation/refactoring flow.

If the repository has not adopted shared conventions and the compatibility resolver is unavailable, state that limitation and use the minimal fallback: make ownership explicit, keep APIs narrow, keep related behavior together, avoid circular dependencies, and do not introduce a coordination layer unless the workload actually needs coordination.
