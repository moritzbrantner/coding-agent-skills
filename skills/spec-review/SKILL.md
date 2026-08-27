---
id: "general/spec-review"
name: "spec-review"
description: "Review a candidate against its applicable ticket and exact parent specification revision without modifying it."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["review", "spec", "acceptance"]
requires: []
related-to: ["general/code-review", "general/to-spec", "general/to-tickets"]
readiness:
  - predicate: "artifact-available"
    artifact: "applicable-spec-or-ticket"
extensions: {}
---

# Spec Review

This is a read-only review axis.

Compare the candidate with the applicable ticket and the exact specification revision/hash to which that ticket is bound. Trace each finding to a stated acceptance outcome, settled constraint, or ticket refinement. Do not add new product requirements and do not treat implementation preferences as spec failures.

Report missing behavior, contradictory behavior, unfulfilled acceptance criteria, scope drift, or evidence gaps separately from engineering-standard findings. If the spec/ticket is stale or contradictory, surface that fact instead of guessing which version should win.

Do not modify code or project documents. Remediation belongs above the review layer.
