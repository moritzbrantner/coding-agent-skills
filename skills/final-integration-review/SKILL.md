---
id: "general/final-integration-review"
name: "final-integration-review"
description: "Perform the final exact-head semantic and mechanical review of a pull request and hand an approved candidate to existing integration mechanics."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["review", "integrate", "merge", "pull-request"]
requires: []
related-to: ["general/prepare-handoff", "general/review-and-fix", "general/code-review", "general/cross-repository-boundary-review"]
readiness:
  - predicate: "tool-available"
    tool: "coding-tooling"
extensions:
  agent.procedure:
    schemaVersion: 1
    routing:
      useWhen: ["A pull request is believed complete and the remaining question is whether the exact current candidate is integration-ready."]
      doNotUseWhen: ["Implementation or review remediation is still actively changing the candidate.", "The task is to perform ordinary code review rather than a final exact-head integration decision."]
      mutates: false
      approvalBoundary: "none"
    termination:
      terminal: true
      doneWhen: ["A fresh mechanical receipt and semantic review produce either an integration-ready decision bound to the exact head/base identities or a concrete blocking decision; the identities are re-resolved immediately before the result is returned."]
      stopWithoutChangeWhen: ["Required checks are skipped, pending, failed, or unavailable.", "The PR is draft, unmergeable, has unresolved review threads or unintegrated stack dependencies, or the reviewed head/base moved.", "A semantic preservation, authority, compatibility, security, persistence, browser/mobile, or performance requirement remains unproved."]
      escalateWhen: ["The candidate is integration-ready but the current caller lacks authority to integrate it.", "A semantic requirement depends on unresolved human product or domain intent."]
      evidenceRequired: ["The exact PR head and base SHAs, fresh mechanical receipt, actual diff, repository policy, relevant task/handoff evidence, and resolved semantic review requirements are available."]
      outOfScope: ["Implementing merge queues or VCS mutation.", "Repairing the candidate inside the final review.", "Caching a prior integration-ready decision after head, base, check, mergeability, stack, or review state changes."]
    artifacts:
      consumes: ["candidate-head", "repository-state", "task-packet", "handoff-receipt", "review-state"]
      produces: ["integration-decision"]
---

# Final Integration Review

Use this when a pull request is believed to be complete and the remaining question is whether the exact candidate may be integrated.

1. Resolve the current PR head SHA and base SHA. Treat both as immutable inputs to this review.
2. Generate the mechanical receipt with those exact identities:

   `coding-tooling pr receipt <number> --expected-head <head-sha> --expected-base <base-sha> --json`

3. Stop unless the receipt passes. Required skipped, pending, failed, or unavailable checks are not green. Draft state, unmergeability, unresolved review threads, an unintegrated stack dependency, a moved head, or unavailable exact-head check evidence remains blocking.
4. Read the repository `AGENTS.md`, installed conventions, task packet/handoff when available, and the actual diff. Resolve every semantic review requirement against evidence. Pay particular attention to:
   - the repository's declared authority owner and prohibited write-back direction;
   - `mustPreserve` and `outOfScope` constraints;
   - public/API/README/Pages claims that must not exceed demonstrated capability;
   - compatibility, persistence, protocol, security, deterministic replay, and browser/mobile boundaries implicated by the change;
   - performance claims, which require equivalent benchmark evidence rather than merely a green functional suite.
5. Re-check review comments after repairs. Do not treat an outdated thread as resolved merely because its line moved; verify the concern no longer applies, reply with the repair evidence, and resolve the thread only then.
6. If the PR head or base changes during the review, discard the integration receipt and repeat the exact-head review from step 1.
7. Immediately before returning an integration-ready decision, re-resolve the current head and base. If either differs from the reviewed identities, repeat from step 1. Otherwise generate a fresh `coding-tooling pr receipt` with the same expected head and base and stop unless it passes. This refresh is required even when no file changed so mutable check, draft/mergeability, stack, and review-thread state is re-evaluated by `coding-tooling` instead of relying on a cached receipt.
8. When the fresh mechanical receipt and semantic review both pass, return an integration-ready decision bound to the exact head SHA. If the caller already has explicit authority to integrate, hand that exact candidate to the repository's existing integration/auto-merge capability without asking for duplicate confirmation. Otherwise return the decision to the caller for the required authority step.

Do not implement merge queues, VCS mutation, check collection, receipt-state tracking, or durable approval state in this skill. Those mechanics remain owned by `coding-tooling`, the hosting platform, or the caller/orchestrator.
