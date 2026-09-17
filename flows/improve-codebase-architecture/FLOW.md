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
extensions:
  agent.procedure:
    schemaVersion: 1
    routing:
      useWhen: ["The task asks to change consequential architecture or module boundaries rather than only assess or design them."]
      doNotUseWhen: ["The request only needs a read-only architecture assessment.", "The target architecture is not yet sufficiently understood to propose a consequential change."]
      mutates: true
      approvalBoundary: "required"
    termination:
      terminal: true
      doneWhen: ["An approved architecture design is implemented, repository verification passes, and the resulting candidate is reviewed."]
      stopWithoutChangeWhen: ["Architecture review or design establishes that no material change is justified.", "The human does not approve the consequential design."]
      escalateWhen: ["Repository verification or final review leaves blocking findings after the bounded implementation pass."]
      evidenceRequired: ["Architecture findings, a concrete design proposal, explicit human approval, repository verification, and final review evidence are available for the implemented change."]
      outOfScope: ["Unapproved architecture migration.", "Durable migration scheduling, retries, or multi-worker orchestration."]
    artifacts:
      consumes: ["repository-state", "installed-policy"]
      produces: ["architecture-findings", "design-proposal", "architecture-change-result", "verification-evidence", "review-findings"]
---

# Improve Codebase Architecture

This flow turns read-only architecture findings into a concrete design and then a behavior-preserving implementation.

Architectural/module-boundary changes are consequential by definition here, so the human approval gate is mandatory before refactoring. The flow does not create migration tasks, schedule workers, or retry failed changes; an outer runtime may decompose a larger migration.
