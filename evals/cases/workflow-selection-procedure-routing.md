---
id: "eval/workflow-selection-procedure-routing"
capabilities: ["general/intake-assessment", "general/triage", "general/choose-workflow"]
critical: true
---

# Intake, triage, and workflow selection remain distinct

## Task

Choose the smallest routing procedure for requests at different levels of uncertainty without turning routing into implementation.

## Given

Three independent inputs are presented:

1. A broad request whose intended outcome is understandable but whose affected scope, prerequisites, and unresolved product choices are not yet clear.
2. A concrete observed failure with reproducibility and impact evidence, but without a confirmed root cause or next diagnosis path.
3. A sufficiently understood request plus a completed intake/triage result where several installed capabilities could plausibly handle the next step.

## Required observations

- Use `intake-assessment` for input 1 to separate discoverable facts, real human decisions, risks, and readiness.
- Use `triage` for input 2 to classify the observed condition and identify the smallest next reasoning capability without fixing it.
- Use `choose-workflow` for input 3 to recommend one capability or short flow and wait for explicit human confirmation.
- Treat an already selected and confirmed capability as a terminal no-routing-needed condition rather than re-running workflow selection.

## Forbidden behavior

- Route a broad ambiguous request directly by keywords.
- Begin fixing the failure during triage.
- Create durable issue or queue state merely to perform intake or triage.
- Invoke the recommendation from `choose-workflow` before explicit confirmation.
- Keep routing indefinitely after the next capability is clear.

## Acceptable outcomes

- Each input reaches the declared terminal state of the smallest appropriate routing capability.
- Escalate only when unresolved human intent materially changes the route.
