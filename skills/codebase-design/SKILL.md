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

Apply the stable design vocabulary from `coding-agent-conventions` to the concrete problem; do not duplicate that doctrine here.

Understand the behavior and ownership boundaries first. Identify where state, policy, data, and side effects naturally belong. Prefer cohesive deep modules, small public surfaces, locality, and progressive composition over forwarding layers or central ontologies.

Consider at least two plausible designs when the choice is consequential. Compare them against coupling, reversibility, operational complexity, testability, migration cost, and how much new abstraction they require. Recommend the smallest design that solves the actual boundary problem.

This skill designs; it does not silently perform a broad architectural migration. Settled consequential choices can be handed to domain/ADR documentation and an implementation/refactoring flow.

Without `coding-agent-conventions`, use the minimal fallback: make ownership explicit, keep APIs narrow, keep related behavior together, avoid circular dependencies, and do not introduce a coordination layer unless the workload actually needs coordination.
