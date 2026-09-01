---
id: "general/fix-bug"
name: "fix-bug"
description: "Diagnose a bug, resolve uncertain intent when necessary, own the regression red-to-green fix, then refactor, verify, and review."
kind: "flow"
maturity: "stable"
entry-point: true
intents: ["bug", "fix", "regression"]
requires: []
related-to: ["general/diagnosing-bugs", "general/browser-investigation", "general/tdd", "general/refactor"]
readiness:
  - predicate: "action-available"
    action: "repository.verify"
flow:
  steps:
    - id: diagnose
      kind: invoke
      capability: "general/diagnosing-bugs"
      output: "diagnosis"
    - id: uncertain-diagnosis
      kind: branch
      condition:
        source: "diagnosis.confidence"
        equals: "uncertain"
      then:
        - id: confirm-intent
          kind: human-gate
          prompt: "Resolve the remaining intended-behavior or diagnosis ambiguity before changing behavior."
      else: []
    - id: red-green-fix
      kind: invoke
      capability: "general/tdd"
      inputs:
        diagnosis: "diagnosis"
      output: "green-fix"
    - id: cleanup
      kind: invoke
      capability: "general/refactor"
      inputs:
        baseline: "green-fix"
      output: "refactor-result"
    - id: browser-verification
      kind: branch
      condition:
        source: "diagnosis.affected-surface"
        equals: "browser"
      then:
        - id: exercise-browser-boundary
          kind: invoke
          capability: "general/browser-investigation"
          inputs:
            diagnosis: "diagnosis"
            baseline: "refactor-result"
          output: "browser-evidence"
      else: []
    - id: verify
      kind: action
      action: "repository.verify"
    - id: review
      kind: invoke
      capability: "general/code-review"
      output: "review-findings"
extensions: {}
---

# Fix Bug

The diagnostic skill may hand over a failing reproducer/test, but this flow decides whether it becomes permanent and owns the actual red-to-green transition.

Browser-visible diagnoses use `affected-surface: browser`, which makes the executable flow invoke `general/browser-investigation` after the fix/refactor and before repository verification. Interactive browser evidence supplements rather than replaces the durable regression test owned by the TDD step.

Before changing code, use the consumer repository's `AGENTS.md` and installed `.conventions/` modules as the policy context. A bug fix should not fetch a newer convention revision mid-run; convention updates are separate deliberate repository changes. Repositories that have not migrated may temporarily use `coding-tooling conventions resolve` as a compatibility fallback.

If diagnosis evidence cannot settle intended behavior, the human gate resolves that ambiguity before code changes. After green, refactor inspection, conditional browser verification, repository verification, and review are required. Any later remediation is a separate bounded caller decision; this flow does not loop.
