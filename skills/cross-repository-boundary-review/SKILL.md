---
id: "general/cross-repository-boundary-review"
name: "cross-repository-boundary-review"
description: "Review a multi-repository change against declared dependency direction, semantic authority, adapters, and exact source-development revisions."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["architecture", "dependency", "authority", "multi-repository"]
requires: []
related-to: ["general/architecture-review", "general/final-integration-review", "general/prepare-task-packet"]
readiness: []
extensions: {}
---

# Cross-Repository Boundary Review

Use this before widening an implementation across repositories or during final review of a change that crosses repository-owned capabilities.

1. Run `coding-tooling fleet authority-graph --root <fleet-root> --json` against the workspace containing the participating repositories.
2. Stop on a failed graph. Duplicate authoritative owners, invalid local-only source graphs, missing required local checkouts, unreadable exact revisions, or revision drift are architecture/evidence blockers rather than reasons to fall back to approximate source state.
3. Inspect the graph's `.repository.toml` dependency edges and the repositories' `AGENTS.md` authority declarations. Identify:
   - the single owner of each semantic capability being changed;
   - adapter/composition repositories that may translate but must not redefine owned semantics;
   - non-authoritative simulations, projections, previews, or comparison implementations;
   - prohibited write-back directions;
   - exact source-development revisions used for the proof.
4. Keep dependency direction consistent with the owning contracts. Do not introduce sideways domain dependencies merely to exchange internal implementation data when a lower contract or explicit adapter is the correct boundary.
5. Bound task expansion by ownership and contract needs, not an arbitrary repository count. If the required source graph keeps expanding, stop and classify it as architecture/migration work rather than recursively repairing unrelated repositories.
6. When a repository has a real semantic authority boundary but no local declaration, derive the likely boundary from existing architecture and surface it as an explicit repository-guidance change for review. Do not invent ownership merely to make graph coverage reach 100%.
7. Carry the relevant authority and exact-revision constraints into the task packet's `mustPreserve`, `outOfScope`, and semantic `reviewRequirements` so later handoff and integration review retain them.

The fleet graph is deterministic evidence about declared ownership and source state; this skill supplies architectural interpretation. It must not create a second dependency graph or override repository-local authority declarations in prose.
