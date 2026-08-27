---
id: "general/code-review"
name: "code-review"
description: "Run standards review and, when applicable, specification review as separate read-only axes."
kind: "flow"
maturity: "stable"
entry-point: true
intents: ["review", "quality", "acceptance"]
requires: []
related-to: ["general/standards-review", "general/spec-review", "general/review-and-fix"]
readiness: []
flow:
  steps:
    - id: review-axes
      kind: parallel
      steps:
        - id: standards
          kind: invoke
          capability: "general/standards-review"
          output: "standards-findings"
        - id: spec-if-applicable
          kind: branch
          condition:
            source: "review-context.has-spec-or-ticket"
            equals: true
          then:
            - id: spec
              kind: invoke
              capability: "general/spec-review"
              output: "spec-findings"
          else: []
extensions: {}
---

# Code Review

Run the review axes independently and keep their findings separate. Standards review always runs. Specification review runs only when an applicable ticket/spec artifact exists; a direct bug fix or bounded change without such an artifact remains reviewable rather than making the whole flow not-ready.

Standards findings must not be disguised as spec failures, and spec failures must trace to the applicable spec/ticket. This flow is read-only. It does not remediate findings; remediation belongs to `review-and-fix` or another caller.
