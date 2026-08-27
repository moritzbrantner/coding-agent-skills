---
id: "general/improve-codebase-architecture"
name: "improve-codebase-architecture"
description: "Review an architecture, design the intended boundary change, obtain approval, then refactor, verify, and review."
kind: "flow"
maturity: "stable"
entry-point: true
intents: ["architecture", "improve", "refactor"]
requires: []
related-to: ["general/architecture-review", "general/codebase-design", "general/refactor"]
readiness:
  - predicate: "action-available"
    action: "repository.verify"
flow:
  steps:
    - id: architecture-review
      kind: invoke
      capability: "general/architecture-review"
      output: "architecture-findings"
    - id: design
      kind: invoke
      capability: "general/codebase-design"
      inputs:
        findings: "architecture-findings"
      output: "proposed-design"
    - id: approve
      kind: human-gate
      prompt: "Approve the consequential architecture/module-boundary change before implementation."
    - id: refactor
      kind: invoke
      capability: "general/refactor"
      inputs:
        approved-design: "proposed-design"
      output: "architecture-change"
    - id: verify
      kind: action
      action: "repository.verify"
    - id: review
      kind: invoke
      capability: "general/code-review"
      output: "review-findings"
extensions: {}
---

# Improve Codebase Architecture

This flow turns read-only architecture findings into a concrete design and then a behavior-preserving implementation.

Architectural/module-boundary changes are consequential by definition here, so the human approval gate is mandatory before refactoring. The flow does not create migration tasks, schedule workers, or retry failed changes; an outer runtime may decompose a larger migration.
