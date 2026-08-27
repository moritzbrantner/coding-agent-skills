---
id: "general/implement"
name: "implement"
description: "Execute an approved bounded change through its testing strategy, refactor inspection, verification, review, bounded remediation, and optional commit."
kind: "flow"
maturity: "stable"
entry-point: true
intents: ["implement", "change", "feature"]
requires: []
related-to: ["general/tdd", "general/refactor", "general/code-review", "general/review-and-fix"]
readiness:
  - predicate: "action-available"
    action: "repository.verify"
flow:
  steps:
    - id: testing-path
      kind: branch
      condition:
        source: "testing-strategy.mode"
        equals: "tdd"
      then:
        - id: red-green
          kind: invoke
          capability: "general/tdd"
          output: "green-implementation"
      else:
        - id: approved-non-tdd-change
          kind: action
          action: "runtime.apply-approved-change"
          optional: true
          fallback: "agent"
    - id: refactor-inspection
      kind: invoke
      capability: "general/refactor"
      inputs:
        baseline: "green-implementation-or-approved-non-tdd-change"
      output: "refactor-result"
    - id: verify
      kind: action
      action: "repository.verify"
    - id: review
      kind: invoke
      capability: "general/code-review"
      output: "first-review"
    - id: remediate
      kind: branch
      condition:
        source: "first-review.blocking"
        equals: true
      then:
        - id: bounded-remediation
          kind: invoke
          capability: "general/review-and-fix"
          inputs:
            findings: "first-review"
          output: "remediation-result"
      else: []
    - id: commit
      kind: action
      action: "vcs.commit"
      optional: true
      fallback: "skip"
extensions: {}
---

# Implement

`implement` is a general executable flow, not a policy-heavy skill. The approved spec/ticket plus the currently resolved `coding-agent-conventions` stack determine the testing strategy and engineering policy.

Before implementation begins, the caller or current agent should obtain the repository's live convention context through `coding-tooling conventions resolve`. Read the returned files plus repository-local instructions; do not substitute a consumer-side copy of shared convention text. If the caller records a run receipt, preserve the resolver's `sourceRevision` as evidence of the policy observed for that run without turning it into a consumer dependency pin.

Behavior changes use TDD when the approved strategy calls for it. Pure refactors, configuration changes, generated artifacts, and similar approved non-TDD work use the explicit `runtime.apply-approved-change` step. A runtime may provide that action; otherwise its declared `agent` fallback means the current agent performs the already-approved bounded edit. This is an execution hook, not a 27th reasoning capability and not a second testing doctrine.

`repository.verify` is the deterministic verification boundary. When the applicable conventions require progressive validation, the runtime should use repository discovery and `coding-tooling` to validate the narrowest useful scope first, broaden only after it is green, and revalidate the smallest affected scope after a broader-scope repair. That behavior belongs to repository policy plus deterministic tooling rather than a second procedural development-loop skill or an unbounded retry loop in this flow.

When an approved change requires unreleased work from another repository, keep the implementation source-first. Respect the dependency-expansion budget from the resolved conventions, pin exact upstream revisions in the consumer's source-dependency declaration, activate the repository's deterministic source-dependency mechanism, and prove the motivating product/application workflow against that exact source graph. Do not publish, bump package versions, or start a release train merely to unblock implementation. Registry-only resolution after source overrides are disabled is a separate release/distribution proof.

Every implementation passes through refactor inspection; `refactor` may return `no-refactor-needed`. Required repository verification follows, then independent code review. One bounded remediation path may run if blocking findings exist. There are no loops/retries in this flow.

The final commit action is optional because commit authority belongs to the caller. An Agent Loop wrapper may replace or surround this with its own candidate/integration mechanics.