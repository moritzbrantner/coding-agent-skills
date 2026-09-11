---
id: "general/review-and-fix"
name: "review-and-fix"
description: "Apply one bounded remediation pass to existing review findings, verify, and review once more."
kind: "flow"
maturity: "stable"
entry-point: true
intents: ["review", "remediate", "fix"]
requires: []
related-to: ["general/code-review", "general/implement"]
readiness:
  - predicate: "artifact-available"
    artifact: "review-findings"
  - predicate: "action-available"
    action: "repository.verify"
flow:
  steps:
    - id: remediation-kind
      kind: branch
      condition:
        source: "findings.behavior-change"
        equals: true
      then:
        - id: behavior-fix
          kind: invoke
          capability: "general/tdd"
          inputs:
            findings: "findings"
          output: "green-behavior-fix"
        - id: behavior-cleanup
          kind: invoke
          capability: "general/refactor"
          inputs:
            baseline: "green-behavior-fix"
          output: "refactor-result"
      else:
        - id: structural-fix
          kind: invoke
          capability: "general/refactor"
          inputs:
            findings: "findings"
          output: "refactor-result"
    - id: verify
      kind: action
      action: "repository.verify"
    - id: rereview
      kind: invoke
      capability: "general/code-review"
      output: "second-review"
extensions: {}
---

# Review And Fix

This flow consumes an existing set of review findings. It performs exactly one explicit remediation pass and one re-review. There is no retry loop.

Do not automatically patch advisory detector output. Remediate only findings that are blocking because repository policy requires them or concrete evidence establishes a defect; preserve unpromoted heuristic findings as residual advice.

Behavior-changing findings return through TDD; behavior-preserving structural findings go through refactoring. If blocking findings remain after the second review, stop and report them to the caller.
