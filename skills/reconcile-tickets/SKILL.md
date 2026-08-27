---
id: "general/reconcile-tickets"
name: "reconcile-tickets"
description: "Reconcile stale spec-bound ticket artifacts through deterministic diff evidence and explicit human decisions."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["tickets", "reconcile", "spec-change"]
requires: ["general/grilling"]
related-to: ["general/to-tickets", "general/grilling"]
readiness:
  - predicate: "artifact-available"
    artifact: "spec-and-ticket-set"
  - predicate: "tool-available"
    tool: "coding-tooling"
extensions: {}
---

# Reconcile Tickets

Invoke this skill explicitly when deterministic tooling reports tickets bound to an older spec revision. Detection may be automatic; reconciliation is not.

1. Ask `coding-tooling` for the spec diff and potentially affected ticket set.
2. Inspect meaning, not only textual overlap.
3. Walk the affected tickets one by one using the `grilling` decision-frontier discipline. Explain what changed and ask only the human decisions that evidence cannot settle.
4. Decide **all affected tickets first**. Do not mutate tickets incrementally.
5. Build and show the complete resulting dependency graph and reconciliation plan.
6. Require final human approval, then apply the approved **ticket-artifact** mutations as one coherent reconciliation.

Preserve unaffected tickets. Completed work is immutable historical fact: never invalidate or reopen a completion receipt because a later spec changed. Create new delta tickets when new work is required.

For an affected ticket that is currently active, return the semantic stale impact and ask the human for the intended disposition: cancel it, let it finish for evidence only, or replace it with revised work. The caller/runtime owns every execution-state transition: marking the run stale, preventing automatic integration, cancellation, evidence-only completion, replacement, or allowing unaffected active work to continue. This skill changes ticket artifacts only after the full reconciliation is approved.

This skill reasons about semantic impact; deterministic diffing, parsing, candidate scope discovery, run state, and integration policy belong outside it.
