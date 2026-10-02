---
id: "general/unblock-issues"
name: "unblock-issues"
description: "Review blocked issues once, resolve discoverable facts, and walk the remaining human decision frontier one question at a time."
kind: "skill"
maturity: "provisional"
entry-point: true
intents: ["issue", "blocked", "unblock", "decision"]
requires: ["general/grilling"]
related-to: ["general/work-next-issue", "general/grilling", "general/triage"]
readiness: []
extensions:
  agent.procedure:
    schemaVersion: 1
    routing:
      useWhen: ["Blocked work exists and the caller wants to identify and resolve the human decisions that prevent progress."]
      doNotUseWhen: ["Actionable implementation work remains available and the caller asked to keep implementing.", "The blocker is purely a still-open technical dependency that needs no human decision."]
      mutates: true
      approvalBoundary: "required"
    termination:
      terminal: true
      doneWhen: ["Every blocked item in the invocation snapshot is either unblocked, waiting on a concrete non-decision dependency or external action, or has had its next unresolved human decision surfaced."]
      stopWithoutChangeWhen: ["No blocked issue in the snapshot requires a human decision.", "The blocked-work source cannot be inspected reliably enough to distinguish decisions from dependencies."]
      escalateWhen: ["A blocker requires credentials, permissions, legal approval, or another external authority the current user cannot supply through a decision."]
      evidenceRequired: ["The blocked-work snapshot, blocker classification, discoverable facts used to avoid unnecessary questions, and any settled decisions written back to the source item are available."]
      outOfScope: ["Implementing unrelated actionable issues.", "Creating a durable question queue.", "Asking batches of downstream questions whose prerequisites are unresolved.", "Inventing tracker states or labels."]
    artifacts:
      consumes: ["blocked-issue-source", "repository-set", "repository-state"]
      produces: ["blocked-issue-snapshot", "decision-frontier", "settled-decisions"]
---

# Unblock Issues

Use this skill as an interactive pass over blocked work. Inspect the blocked set once at invocation start, then work from that snapshot unless a material state change makes it stale.

1. Build the blocked-issue snapshot from the configured source. For each item, inspect the issue body, relevant comments or linked work, and owning repository evidence needed to understand the blocker.
2. Classify each blocker as one of three kinds: an unresolved technical dependency, an external/manual action, or a human decision about intended behavior, product, domain, architecture, testing, scope, or acceptance.
3. Resolve discoverable facts yourself. Do not ask the human to look up repository state, existing behavior, CI evidence, linked issues, or other information available through tools.
4. Leave a still-open technical dependency alone and preserve its link. Do not turn it into a human question merely because the issue is blocked.
5. For an external/manual action that contains no decision, surface the smallest concrete action required. Do not disguise an action request as a design question.
6. For genuine human decisions, use `general/grilling` to build a dependency-aware decision frontier across the snapshot. Ask exactly one currently answerable question per assistant turn. Include only the concise evidence, alternatives, and consequences needed for that decision. Do not ask downstream questions early.
7. After the human answers, record the settled decision on the originating issue or in the repository's existing durable decision location when appropriate. Update the issue's blocker state using existing tracker conventions only if the answer actually resolves that blocker.
8. Recompute the decision frontier from the same snapshot after each answer. Re-scan the blocked source only when an answer or external change materially invalidates the snapshot.
9. Continue one question at a time until the frontier is empty or the user stops the interaction.

A persistent implementation Goal should normally invoke this skill only after `general/work-next-issue` reports that no actionable issue remains. The unblock skill does not implement the newly unblocked work itself; the implementation Goal can resume its normal issue scan afterward.

Do not maintain claims, leases, retry counters, or a private durable queue. The issue tracker and repository remain the durable sources of truth; the question frontier exists only in the interactive session.
