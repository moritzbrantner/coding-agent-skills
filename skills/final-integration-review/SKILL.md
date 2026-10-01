---
id: "general/final-integration-review"
name: "final-integration-review"
description: "Review a pull request's code, CI result, and merge blockers before integration."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["review", "integrate", "merge", "pull-request"]
requires: []
related-to: ["general/prepare-handoff", "general/review-and-fix", "general/code-review", "general/cross-repository-boundary-review"]
readiness: []
extensions:
  agent.procedure:
    schemaVersion: 1
    routing:
      useWhen: ["A pull request is believed complete and the remaining question is whether it is ready to integrate."]
      doNotUseWhen: ["Implementation or review remediation is still actively changing the candidate.", "The task is to perform ordinary code review rather than a final integration decision."]
      mutates: false
      approvalBoundary: "none"
    termination:
      terminal: true
      doneWhen: ["The current GitHub checks and semantic review produce an integration-ready decision or a concrete blocking decision."]
      stopWithoutChangeWhen: ["Required checks are pending or failed.", "The PR is draft, unmergeable, has unresolved review threads or unintegrated stack dependencies.", "A semantic preservation, authority, compatibility, security, persistence, browser/mobile, or performance requirement remains unproved."]
      escalateWhen: ["The candidate is integration-ready but the current caller lacks authority to integrate it.", "A semantic requirement depends on unresolved human product or domain intent."]
      evidenceRequired: ["Current PR status and checks, actual diff, repository policy, relevant task evidence, and resolved semantic review requirements are available."]
      outOfScope: ["Implementing merge queues or VCS mutation.", "Repairing the candidate inside the final review.", "Repeating CI or verifying runner identity to re-prove GitHub's checks."]
    artifacts:
      consumes: ["candidate-head", "repository-state", "task-packet", "handoff-receipt", "review-state"]
      produces: ["integration-decision"]
---

# Final Integration Review

Use this when a pull request is believed complete and the remaining question is whether it may be integrated.

1. Read the current pull request, its actual diff, required GitHub checks, mergeability, review threads, and any declared stack dependencies. Trust GitHub's current check status; do not launch another exact-head verification or runner-identity check.
2. Reuse the task’s [resolved policy context](../../docs/policy-context.md) and relevant task context. Review authority boundaries, preservation requirements, compatibility, persistence, protocol, security, browser/mobile behavior, and performance claims implicated by the change. Require representative evidence for material claims.
3. Resolve real review findings. After a repair, inspect the changed concern and the normal CI result. Do not infer that an outdated thread has been resolved merely because its line moved.
4. Before integration, use GitHub's current mergeability and required-check result. A merge precondition may guard against concurrent head movement; it does not require rerunning validation.
5. Return the integration decision and any blockers. If the caller has authority to integrate, use the repository's existing merge mechanism without asking again.

Keep CI execution, merge queues, VCS mutation, and durable approval state in their existing owners.
