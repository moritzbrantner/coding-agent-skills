---
id: "general/intake-assessment"
name: "intake-assessment"
description: "Assess an incoming coding request for scope, uncertainty, risk, and missing prerequisites before selecting work."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["intake", "assess", "scope"]
requires: []
related-to: ["general/choose-workflow", "general/triage", "general/grilling"]
readiness: []
extensions: {}
---

# Intake Assessment

Inspect the request and repository enough to separate known facts, discoverable facts, unresolved human decisions, and execution prerequisites.

Produce a compact assessment: intended outcome, apparent scope, likely affected semantic areas, meaningful risks, missing evidence, and whether the request is ready for direct implementation, diagnosis, design, specification, or further grilling.

Do not create tasks, issues, or durable state. Do not ask the user for repository facts you can discover. Do not select a workflow merely by keyword; base the assessment on the actual uncertainty and risk.

`choose-workflow` may consume this assessment to make an advisory routing recommendation.
