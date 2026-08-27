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
6. Require final human approval, then apply the ticket mutations as one coherent reconciliation.

Preserve unaffected tickets. Completed work is immutable historical fact: never invalidate or reopen a completion receipt because a later spec changed. Create new delta tickets when new work is required.

If an affected ticket is currently active, mark the execution as stale through the caller/runtime and surface the human choices: cancel it, let it finish for evidence only, or replace it with revised work. A stale active ticket must not integrate automatically. Unaffected active work may continue.

This skill reasons about semantic impact; deterministic diffing, parsing, and candidate scope discovery belong in `coding-tooling`.
