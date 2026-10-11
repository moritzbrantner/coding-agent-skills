---
id: "general/to-spec"
name: "to-spec"
description: "Turn settled intent and repository evidence into one canonical implementation-aware Markdown specification."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["spec", "requirements", "planning"]
requires: []
related-to: ["general/grilling", "general/spec-review"]
readiness: []
extensions: {}
---

# To Spec

Produce the canonical specification as human-readable Markdown. Do not create a separately editable JSON/YAML model. Deterministic tooling may parse or snapshot the Markdown later, but that projection is derived evidence only.

**Sequence:** Before specifying a new product or major feature, check existing owner-approved direction (including its Atlas when present). Use `grilling` **only if a material owner choice remains unresolved**; when direction is already settled, proceed directly to the implementation-aware spec. Well-scoped issues and routine fixes need no extra design exercise.

## Rules

- Give every spec a stable immutable spec ID.
- Use the repository-configured spec root; default to `docs/specs/`.
- Do not require or publish an issue.
- Do not invent missing product/domain/architecture decisions. Retrieve applicable owner Decisions/Questions (scoped `daq` when available, otherwise Markdown). If a material choice is unresolved, return to `grilling` **before** preparing implementation scope; do not substitute a guessed MVP.
- Reference Atlas IDs and current decisions where an Atlas is authoritative; use specs for scope, scenarios, acceptance, and implementation-relevant detail, not another editable list of product decisions.
- Make the spec implementation-aware but not prescriptive. Record settled API/schema/module-boundary or architectural commitments that constrain the solution; omit details an implementation agent can cheaply infer.
- Derive testing strategy primarily from repository test structure, resolved convention modules, and deterministic repository discovery. Ask the human only when there is a genuine testing or architecture choice.
- Generic testing doctrine belongs in conventions, not in every spec.

A useful spec states goals, non-goals, settled behavior, constraints, acceptance outcomes, consequential architecture commitments, and the chosen verification strategy. Acceptance outcomes should be concrete enough for later ticket decomposition and spec review.

Reuse the task’s [resolved policy context](../../docs/policy-context.md) for applicable engineering doctrine and repository-local exceptions.
