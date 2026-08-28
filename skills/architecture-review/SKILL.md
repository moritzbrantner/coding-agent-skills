---
id: "general/architecture-review"
name: "architecture-review"
description: "Review an existing codebase architecture and report boundary, coupling, and ownership findings without changing code."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["architecture", "review", "design"]
requires: []
related-to: ["general/codebase-design", "general/improve-codebase-architecture"]
readiness: []
extensions: {}
---

# Architecture Review

This is a read-only architecture review.

Inspect module/service boundaries, dependency direction, public surface area, state ownership, duplicated policy, cross-cutting coupling, and whether abstractions correspond to real independent concepts. Compare the code to settled ADR/domain knowledge, repository-local guidance, and the design doctrine in the installed `.conventions/` modules.

Review against the policy committed to the repository rather than fetching a newer central policy revision. `coding-tooling conventions check` may verify the managed snapshots against their lock. Use live `coding-tooling conventions resolve` only as a migration fallback when installed convention modules are unavailable.

Report specific findings with evidence and likely consequence. Distinguish local cleanup from a genuinely consequential redesign. Do not recommend a large migration merely because a different shape is aesthetically cleaner.

When docs and code disagree about intended architecture, investigate and surface the contradiction rather than declaring one side authoritative.

The output should be usable by `codebase-design` or `improve-codebase-architecture`; this skill does not mutate code. If neither installed conventions nor the compatibility resolver are available, state that limitation rather than silently assuming project-specific preferences.
