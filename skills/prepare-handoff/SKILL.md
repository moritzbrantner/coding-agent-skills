---
id: "general/prepare-handoff"
name: "prepare-handoff"
description: "Produce an exact-head machine-verified handoff for a bounded repository change without claiming unresolved semantic review requirements are proved."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["handoff", "continue", "verify", "agent"]
requires: []
related-to: ["general/prepare-task-packet", "general/final-integration-review", "general/review-and-fix"]
readiness:
  - predicate: "tool-available"
    tool: "coding-tooling"
extensions:
  agent.procedure:
    schemaVersion: 1
    routing:
      useWhen: ["A bounded implementation is complete enough to transfer to another run, reviewer, or integration caller from exact-head evidence rather than conversational reconstruction."]
      doNotUseWhen: ["The candidate is still being implemented or has an uncommitted working tree.", "The task packet or exact candidate identity is unavailable."]
      mutates: true
      approvalBoundary: "none"
    termination:
      terminal: true
      doneWhen: ["Exact-head verification passes and a handoff receipt bound to the current candidate SHA and task-packet digest is generated, with unresolved deterministic findings and pending semantic review requirements surfaced to the caller."]
      stopWithoutChangeWhen: ["The candidate working tree is dirty or uncommitted.", "Verification is failed, error, or unavailable.", "Candidate HEAD or task-packet identity moved and stale evidence must be discarded."]
      escalateWhen: ["A pending semantic review requirement must be resolved before integration but cannot be settled by mechanical evidence."]
      evidenceRequired: ["The validated task packet, exact candidate SHA, exact-head verification report, environment identity, and bound handoff receipt are available."]
      outOfScope: ["Resolving semantic review requirements.", "Integrating or publishing the candidate.", "Persisting run history or durable continuation state."]
    artifacts:
      consumes: ["task-packet", "candidate-head", "repository-state"]
      produces: ["verification-evidence", "handoff-receipt"]
---

# Prepare Handoff

Use this when implementation is complete enough that another run, reviewer, or integration caller should be able to continue from evidence rather than rediscovering the work.

1. Require the bounded task packet produced by `general/prepare-task-packet` and a committed, clean candidate HEAD. Do not create a handoff from an uncommitted working tree.
2. Run exact-head verification and persist only the ephemeral report:

   `coding-tooling agent verify .artifacts/coding-tooling/task.json --report .artifacts/coding-tooling/verification.json --json`

3. Stop unless verification returns `passed`. `failed`, `error`, or `unavailable` evidence remains a blocker; never convert a missing capability or moved HEAD into a successful handoff.
4. Produce the handoff receipt:

   `coding-tooling agent handoff .artifacts/coding-tooling/task.json --verification-report .artifacts/coding-tooling/verification.json --report .artifacts/coding-tooling/handoff.json --json`

5. Confirm that the receipt is bound to the current candidate SHA and task-packet digest. If HEAD or the packet changes, discard the stale verification/handoff and regenerate them.
6. Surface the receipt's changed files, environment identity, unresolved deterministic findings, and strongest next action to the caller.
7. Treat `semanticReview.required` as pending reasoning work. A green machine handoff deliberately does not mark authority, architecture, public-claim scope, or other semantic review requirements as resolved.

Keep the packet and receipts under ignored `.artifacts/`. Do not commit them or turn this skill into run-history storage; the caller/orchestrator owns continuation state beyond the current handoff.
