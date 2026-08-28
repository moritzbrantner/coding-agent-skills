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

`implement` is a general executable flow, not a policy-heavy skill. The approved spec/ticket, repository-local guidance, and installed convention modules determine the testing strategy and engineering policy.

Before implementation begins, read the repository's `AGENTS.md` and `.conventions/index.md` when present, then load only the convention modules relevant to the change. `conventions.lock.json` records which shared-policy revision is installed. Do not fetch the live shared conventions repository merely to begin ordinary implementation. For a repository that has not migrated yet, `coding-tooling conventions resolve` remains a temporary compatibility fallback.

Behavior changes use TDD when the approved strategy calls for it. Pure refactors, configuration changes, generated artifacts, and similar approved non-TDD work use the explicit `runtime.apply-approved-change` step. A runtime may provide that action; otherwise its declared `agent` fallback means the current agent performs the already-approved bounded edit. This is an execution hook, not another reasoning capability and not a second testing doctrine.

`repository.verify` is the deterministic verification boundary. When the installed conventions require progressive validation, use repository discovery and `coding-tooling` to validate the narrowest useful scope first, broaden only after it is green, and revalidate the smallest affected scope after a broader-scope repair. Convention-installation integrity may be checked with `coding-tooling conventions check`; that check does not replace normal repository verification.

When an approved change requires unreleased work from another repository, keep the implementation source-first. Respect repository policy, pin exact upstream revisions in the consumer's source-dependency declaration, activate the deterministic source-dependency mechanism, and prove the motivating workflow against that source graph. Do not publish or bump package versions merely to unblock implementation. Registry-only resolution after source overrides are disabled remains a separate release/distribution proof.

Every implementation passes through refactor inspection; `refactor` may return `no-refactor-needed`. Required repository verification follows, then independent code review. One bounded remediation path may run if blocking findings exist. There are no loops/retries in this flow.

The final commit action is optional because commit authority belongs to the caller. An Agent Loop wrapper may replace or surround this with its own candidate/integration mechanics.
