---
id: "general/to-spec"
name: "to-spec"
description: "Turn settled intent and repository evidence into one canonical implementation-aware Markdown specification."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["spec", "requirements", "planning"]
requires: []
related-to: ["general/grilling", "general/to-tickets", "general/spec-review"]
readiness: []
extensions: {}
---

# To Spec

Produce the canonical specification as human-readable Markdown. Do not create a separately editable JSON/YAML model. Deterministic tooling may parse or snapshot the Markdown later, but that projection is derived evidence only.

## Rules

- Give every spec a stable immutable spec ID.
- Use the repository-configured spec root; default to `docs/specs/`.
- Do not require or publish an issue.
- Do not invent missing product/domain/architecture decisions. If a material decision is unresolved, route back through `grilling`, resolve it, then resume the spec.
- Make the spec implementation-aware but not prescriptive. Record settled API/schema/module-boundary or architectural commitments that constrain the solution; omit details an implementation agent can cheaply infer.
- Derive testing strategy primarily from repository test structure, `coding-agent-conventions`, and deterministic repository discovery. Ask the human only when there is a genuine testing or architecture choice.
- Generic testing doctrine belongs in conventions, not in every spec.

A useful spec states goals, non-goals, settled behavior, constraints, acceptance outcomes, consequential architecture commitments, and the chosen verification strategy. Acceptance outcomes should be concrete enough for later ticket decomposition and spec review.

If `coding-agent-conventions` is unavailable, use public behavior seams, existing repository test structure, and the smallest testing strategy that can demonstrate the requested behavior.
