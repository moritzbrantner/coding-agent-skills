---
id: "general/resolve-merge-conflicts"
name: "resolve-merge-conflicts"
description: "Analyze semantic merge conflicts, obtain human intent when needed, apply the approved resolution, verify, review, and optionally commit."
kind: "flow"
maturity: "stable"
entry-point: true
intents: ["merge", "conflict", "resolve"]
requires: []
related-to: ["general/conflict-resolution", "general/code-review"]
readiness:
  - predicate: "artifact-available"
    artifact: "conflicting-changes"
  - predicate: "action-available"
    action: "repository.verify"
flow:
  steps:
    - id: analyze
      kind: invoke
      capability: "general/conflict-resolution"
      output: "resolution-plan"
    - id: ambiguity
      kind: branch
      condition:
        source: "resolution-plan.requires-human"
        equals: true
      then:
        - id: confirm
          kind: human-gate
          prompt: "Choose the intended combined behavior for the ambiguous conflict."
      else: []
    - id: apply
      kind: invoke
      capability: "general/conflict-resolution"
      inputs:
        approved-resolution: "resolution-plan"
      output: "resolved-tree"
    - id: verify
      kind: action
      action: "repository.verify"
    - id: review
      kind: invoke
      capability: "general/code-review"
      output: "review-findings"
    - id: commit
      kind: action
      action: "vcs.commit"
      optional: true
      fallback: "skip"
extensions: {}
---

# Resolve Merge Conflicts

First analyze semantic intent; only then apply the selected resolution. Mechanical conflict-marker removal is not enough.

Verification and review are required after the resolved tree is produced. Commit is optional and remains controlled by caller authority. This flow does not merge/rebase the branch itself and does not own durable VCS orchestration.
