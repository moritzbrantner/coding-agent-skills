---
id: "general/grill-with-docs"
name: "grill-with-docs"
description: "Resolve a decision frontier, persist settled domain knowledge, and end with explicit human confirmation."
kind: "flow"
maturity: "stable"
entry-point: true
intents: ["clarify", "domain", "documentation"]
requires: []
related-to: ["general/grilling", "general/domain-modeling", "general/to-spec"]
readiness: []
flow:
  steps:
    - id: grill
      kind: invoke
      capability: "general/grilling"
      output: "settled-decisions"
    - id: document
      kind: invoke
      capability: "general/domain-modeling"
      inputs:
        decisions: "settled-decisions"
      output: "domain-updates"
    - id: confirm
      kind: human-gate
      prompt: "Confirm the settled decisions and durable documentation before downstream work continues."
extensions: {}
---

# Grill With Docs

This human-facing flow composes `grilling` with `domain-modeling`. The reasoning remains in those skills.

Use it to settle product or architectural direction **before** new major-feature specifications or implementation issues are prepared. A work loop invokes grilling later only when an important new owner choice blocks already-approved work. Routine well-specified fixes do not need this flow.

`domain-modeling` writes only settled knowledge and reports each durable change. This flow always ends with an explicit human confirmation gate before a downstream spec, design, or implementation flow proceeds.
