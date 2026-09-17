---
id: "general/fix-bug"
name: "fix-bug"
description: "Diagnose a bug, stop on unresolved cause, own the regression red-to-green fix, then refactor, verify, and review."
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
    - id: unresolved-diagnosis
      kind: branch
      condition:
        source: "diagnosis.confidence"
        equals: "unresolved"
      then: []
      else:
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
            source: "diagnosis.payload.affectedSurface"
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
extensions:
  agent.procedure:
    schemaVersion: 1
    routing:
      useWhen: ["A correctness bug needs both evidence-backed diagnosis and an owned repository repair through red-to-green verification."]
      doNotUseWhen: ["The request asks only for diagnosis without a fix.", "A trusted diagnosis is explicitly unresolved or the primary problem is performance rather than incorrect behavior."]
      mutates: true
      approvalBoundary: "none"
    termination:
      terminal: true
      doneWhen: ["A resolved diagnosis is repaired through a regression red-to-green transition, cleanup, required browser verification when applicable, repository verification, and final review; or an unresolved diagnosis stops immediately after diagnosis before behavior changes."]
      stopWithoutChangeWhen: ["The diagnosis confidence is `unresolved`.", "The evidence indicates that a new outer investigation is required before a safe repair can be selected."]
      escalateWhen: ["The unresolved diagnosis depends on genuinely missing human intent or external facts that a later outer invocation must obtain.", "Verification or final review leaves blocking findings after the bounded repair pass."]
      evidenceRequired: ["The diagnosis envelope, regression evidence, repository verification, and final review are available for an applied repair; browser evidence is also available when `payload.affectedSurface` is `browser`."]
      outOfScope: ["Guessing a fix from an unresolved diagnosis.", "Retry loops or durable remediation scheduling after the bounded pass."]
    artifacts:
      consumes: ["request-context", "repository-state"]
      produces: ["diagnosis-envelope", "bug-fix-result", "browser-evidence", "verification-evidence", "review-findings"]
---

# Fix Bug

The diagnostic skill may hand over a failing reproducer/test, but this flow decides whether it becomes permanent and owns the actual red-to-green transition.

The shared diagnosis confidence values are `confirmed`, `probable`, and `unresolved`. An `unresolved` diagnosis is a fail-closed terminal path for this invocation: the branch ends immediately after diagnosis and does not mutate behavior. If genuinely missing intended behavior or an external fact is the blocker, resolve that outside this bounded fix flow and start a new diagnosis/fix attempt with stronger evidence.

Resolved browser-visible diagnoses use `payload.affectedSurface: browser`, which makes the executable flow invoke `general/browser-investigation` after the fix/refactor and before repository verification. Interactive browser evidence supplements rather than replaces the durable regression test owned by the TDD step.

Before changing code, use the consumer repository's `AGENTS.md` and installed `.conventions/` modules as the policy context. A bug fix should not fetch a newer convention revision mid-run; convention updates are separate deliberate repository changes. Repositories that have not migrated may temporarily use `coding-tooling conventions resolve` as a compatibility fallback.

After green, refactor inspection, conditional browser verification, repository verification, and review are required. Any later remediation is a separate bounded caller decision; this flow does not loop.
