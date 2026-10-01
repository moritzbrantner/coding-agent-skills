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
extensions:
  agent.procedure:
    schemaVersion: 1
    routing:
      useWhen: ["A concrete architecture or module-boundary problem is established and viable target structures need to be compared."]
      doNotUseWhen: ["The task is only to determine whether an architecture problem exists.", "A target design is already approved and only implementation remains."]
      mutates: false
      approvalBoundary: "none"
    termination:
      terminal: true
      doneWhen: ["Plausible designs and tradeoffs have been compared and the smallest justified design is identified."]
      stopWithoutChangeWhen: ["The current design already satisfies the established need and no consequential redesign is justified."]
      escalateWhen: ["The preferred design depends on unresolved product, domain, compatibility, or operational intent."]
      evidenceRequired: ["The established behavior and ownership problem, repository policy, relevant constraints, and consequential alternatives were considered."]
      outOfScope: ["Implementing the selected design.", "Inventing a redesign without an established boundary problem."]
    artifacts:
      consumes: ["repository-state", "resolved-policy-context", "architecture-findings"]
      produces: ["design-proposal"]
---

# Codebase Design

Reuse the task’s [resolved policy context](../../docs/policy-context.md) for applicable engineering doctrine and repository-local exceptions. Apply that vocabulary to the concrete problem without duplicating doctrine here.

Understand the behavior and ownership boundaries first. Identify where state, policy, data, and side effects naturally belong. Prefer cohesive deep modules, small public surfaces, locality, and progressive composition when those principles are part of the applicable policy.

Consider at least two plausible designs when the choice is consequential. Compare them against coupling, reversibility, operational complexity, testability, migration cost, and how much new abstraction they require. Recommend the smallest design that solves the actual boundary problem.

This skill designs; it does not silently perform a broad architectural migration. Settled consequential choices can be handed to domain/ADR documentation and an implementation/refactoring flow.

When current policy access is unavailable, report the limitation through the policy-context procedure. Evidence-based design advice can continue, but must not claim shared-policy conformance.
