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
extensions:
  agent.procedure:
    schemaVersion: 1
    routing:
      useWhen: ["An incoming request needs its scope, uncertainty, risks, and prerequisites assessed before choosing the next reasoning or implementation capability."]
      doNotUseWhen: ["The observed problem is already concrete enough for triage or diagnosis.", "The task is already bounded and ready for a known implementation flow."]
      mutates: false
      approvalBoundary: "none"
    termination:
      terminal: true
      doneWhen: ["Known facts, discoverable facts, unresolved decisions, execution prerequisites, risks, and the request's readiness state are distinguished."]
      stopWithoutChangeWhen: ["The request is already sufficiently bounded and ready for a known next capability."]
      escalateWhen: ["A necessary product, domain, architecture, testing, or behavior choice cannot be settled from available evidence."]
      evidenceRequired: ["The request and enough repository context were inspected to separate discoverable facts from genuine human decisions."]
      outOfScope: ["Creating tasks or durable queue state.", "Implementing the request.", "Selecting a workflow solely from keywords."]
    artifacts:
      consumes: ["request-context", "repository-state"]
      produces: ["intake-assessment"]
---

# Intake Assessment

Inspect the request and repository enough to separate known facts, discoverable facts, unresolved human decisions, and execution prerequisites.

Produce a compact assessment: intended outcome, apparent scope, likely affected semantic areas, meaningful risks, missing evidence, and whether the request is ready for direct implementation, diagnosis, design, specification, or further grilling.

Do not create tasks, issues, or durable state. Do not ask the user for repository facts you can discover. Do not select a workflow merely by keyword; base the assessment on the actual uncertainty and risk.

`choose-workflow` may consume this assessment to make an advisory routing recommendation.
