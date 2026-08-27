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

Apply the stable design vocabulary from the current resolved `coding-agent-conventions` stack to the concrete problem; do not duplicate that doctrine here. When shared conventions are available, obtain them through `coding-tooling conventions resolve` and read the returned files together with repository-local instructions.

Understand the behavior and ownership boundaries first. Identify where state, policy, data, and side effects naturally belong. Prefer cohesive deep modules, small public surfaces, locality, and progressive composition over forwarding layers or central ontologies when those principles are part of the applicable policy.

Consider at least two plausible designs when the choice is consequential. Compare them against coupling, reversibility, operational complexity, testability, migration cost, and how much new abstraction they require. Recommend the smallest design that solves the actual boundary problem.

This skill designs; it does not silently perform a broad architectural migration. Settled consequential choices can be handed to domain/ADR documentation and an implementation/refactoring flow.

If live convention resolution is unavailable, state that limitation and use the minimal fallback: make ownership explicit, keep APIs narrow, keep related behavior together, avoid circular dependencies, and do not introduce a coordination layer unless the workload actually needs coordination.