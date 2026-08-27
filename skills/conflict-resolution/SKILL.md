---
id: "general/conflict-resolution"
name: "conflict-resolution"
description: "Reason about competing code/content changes and produce or apply an intent-preserving resolution without owning VCS integration."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["conflict", "merge", "resolve"]
requires: []
related-to: ["general/resolve-merge-conflicts", "general/code-review"]
readiness:
  - predicate: "artifact-available"
    artifact: "conflicting-changes"
extensions: {}
---

# Conflict Resolution

Resolve semantic intent, not just conflict markers.

Inspect both sides, their surrounding history/context, tests, specs, and current intended behavior. Classify conflicts as mechanical, independent additive changes, overlapping implementation, or genuine intent disagreement.

On the first pass, produce a resolution plan and flag whether human intent is required. Never discard one side simply because it is older or smaller.

If an approved resolution and write authority are supplied, apply the working-tree edits needed to realize that resolution. Do not commit, merge, rebase, or publish; those are caller/runtime responsibilities. Preserve both sides when compatible and re-run focused checks when edits are applied.

If evidence cannot determine the intended combined behavior, stop for a human decision.
