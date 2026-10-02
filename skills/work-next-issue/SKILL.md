---
id: "general/work-next-issue"
name: "work-next-issue"
description: "Select the first actionable issue from a configured repository set, complete one bounded issue end to end, and return without creating durable orchestration state."
kind: "skill"
maturity: "provisional"
entry-point: true
intents: ["issue", "continue", "work", "repository"]
requires: ["general/implement"]
related-to: ["general/continue-next-slice", "general/triage", "general/implement", "general/unblock-issues"]
readiness: []
extensions:
  agent.procedure:
    schemaVersion: 1
    routing:
      useWhen: ["A caller wants one autonomous issue-work iteration across one or more available repositories."]
      doNotUseWhen: ["A specific issue or bounded task is already selected.", "The request is to resolve human decisions in blocked work rather than implement actionable work."]
      mutates: true
      approvalBoundary: "none"
    termination:
      terminal: true
      doneWhen: ["Exactly one issue is completed, reconciled as already satisfied, or newly blocked by a concrete prerequisite and that prerequisite is durably linked at its proper owner."]
      stopWithoutChangeWhen: ["No actionable issue exists in the current scan.", "The configured issue source or owning repository cannot be inspected reliably enough to select work without guessing."]
      escalateWhen: ["The selected issue depends on unresolved human product, domain, architecture, testing, or behavior intent."]
      evidenceRequired: ["The current issue scan, selected issue, owning repository state, repository-owned verification, and resulting issue update are available."]
      outOfScope: ["Maintaining claims or leases.", "Persisting a private queue, priority score, retry counter, or run history.", "Processing a second independent issue in the same invocation."]
    artifacts:
      consumes: ["issue-source", "repository-set", "repository-state", "session-considered-issues"]
      produces: ["issue-selection", "implementation-result", "session-considered-issues"]
---

# Work Next Issue

Use this skill for one issue-work iteration. A persistent caller such as a Goal may invoke it repeatedly; this skill itself is not a scheduler or durable loop.

1. Refresh the configured open-issue view before selecting work. The source may be GitHub, another tracker, or a caller-provided issue set. Preserve source order unless the caller supplied a different explicit ordering; do not invent a hidden priority score.
2. Reuse a caller/session-local considered cache only to avoid repeating unchanged inspection work. Key each cached classification to the issue revision plus the observed blocker/dependency state. Invalidate it whenever the issue, a linked blocker, or relevant repository state changes; after any tracker mutation in this skill, begin the next invocation from a fresh scan. This cache is ephemeral. Do not write claim labels, lease records, assignment metadata, or a second queue merely to remember that an item was inspected.
3. Inspect issues in order until one is actionable or the first issue requiring a source mutation is found. An issue is not actionable when a required dependency is still open, a consequential human decision is unresolved, another active implementation already owns the same work, or the owning repository is unavailable. An apparently already-satisfied issue is a reconciliation candidate rather than a skip.
4. When the first reconciliation candidate is already satisfied, verify that fact from repository and tracker evidence, update or close the source issue appropriately, and return immediately with `reconciled`. Do not continue to another issue in the same invocation.
5. Enter the issue's owning repository and read its local instructions, installed convention selection, architecture boundaries, and repository-owned validation entrypoints before making implementation decisions. Reuse resolved policy context when the caller already has it.
6. Treat cross-repository ownership as a boundary, not as a reason to duplicate work. If progress requires a change owned by another repository, first search for an existing matching issue there. Reuse and link it when present; otherwise create the smallest clear prerequisite issue at that owner. Link the original issue to the prerequisite and classify the original as blocked using its existing tracker conventions only when the dependency genuinely prevents progress. Do not create orchestration-only tickets.
7. For actionable implementation, hand the bounded issue to `general/implement`. Keep the issue's acceptance criteria, preservation constraints, and repository-local rules authoritative. Validate progressively and finish with the repository-owned completion gate.
8. Update or close the source issue only when the implemented result and required verification support that state. Preserve relevant evidence or links without adding workflow noise.
9. Return after this one issue is completed, reconciled, or newly blocked. The caller must refresh the global issue view before invoking the skill again.

When the scan contains no actionable issue, return `no-actionable-issue` with a compact reason summary. A persistent Goal may then invoke `general/unblock-issues` once to surface human decisions.

This skill deliberately has no claim system. If a future caller introduces concurrent workers, coordination belongs to that caller or an orchestrator rather than being smuggled into the reusable procedure.
