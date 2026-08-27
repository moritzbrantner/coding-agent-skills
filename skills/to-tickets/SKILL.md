---
id: "general/to-tickets"
name: "to-tickets"
description: "Decompose an approved spec into individual traceable Markdown ticket artifacts and a dependency graph."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["tickets", "decompose", "planning"]
requires: []
related-to: ["general/to-spec", "general/reconcile-tickets"]
readiness:
  - predicate: "artifact-available"
    artifact: "canonical-spec"
  - predicate: "tool-available"
    tool: "coding-tooling"
extensions: {}
---

# To Tickets

Decompose one canonical spec revision into individual Markdown ticket artifacts. There is no monolithic `TICKETS.md` and no ticket status state machine.

## Ticket design

- Give every ticket a stable immutable ticket ID plus a human-readable title/slug.
- Bind each ticket to the parent spec stable ID and the exact spec content hash/revision used for decomposition.
- Prefer tracer-bullet vertical slices: one narrow, complete, independently verifiable outcome through the relevant layers.
- Use horizontal decomposition only for a concrete reason.
- Size by coherent outcome first, practical bounded execution second. Split when unrelated changes, scope, or reasoning burden make one agent run impractical; do not use arbitrary token limits.
- Ticket acceptance criteria are traceable refinements of parent spec outcomes. They may add narrower verification needed for the slice but cannot add product requirements.
- Dependencies use hard `blocked-by` edges and soft `prefer-after` edges. Only hard blockers constrain readiness.
- Tickets may name stable semantic areas such as `billing` or `audio-analysis`, not fragile file paths. Deterministic tooling/caller resolves concrete write scope before execution.
- Recognize wide refactors, but do not invent expand/migrate/contract decomposition here. Hand those to the specialized refactoring capability/runtime.

Construct and show the **complete proposed graph** before persistence. On direct human invocation, require human approval of the graph before writing ticket artifacts. An Agent Loop wrapper may omit that gate only when its surrounding flow already explicitly authorized decomposition of the approved spec.

The artifact format is deliberately compatible with the structural lifecycle we chose: a caller may treat an existing ticket as pending work, represent active work through its own run/worktree, and record successful completion with an immutable receipt before removing the ticket. **This skill does none of those lifecycle transitions.** It only produces the approved ticket artifacts and dependency graph; activation, deletion, completion receipts, integration, and execution state belong to the caller/runtime. Do not add `planned/active/done` fields to compensate.

Use deterministic `coding-tooling` parsing/validation rather than creating a second structured source of truth.
