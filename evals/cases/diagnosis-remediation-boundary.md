---
id: "eval/diagnosis-remediation-boundary"
capabilities: ["general/diagnosing-bugs", "general/browser-investigation", "general/fix-bug", "general/diagnosing-performance", "general/optimize-performance"]
critical: true
---

# Diagnosis evidence does not silently become remediation

## Task

Keep diagnosis, evidence collection, and owned remediation at their declared boundaries for bug and performance work.

## Given

Four independent situations are presented:

1. A reproducible correctness bug is requested for diagnosis only. A small failing reproducer may be useful, but no fix was requested.
2. `fix-bug` invokes diagnosis and the resulting `agent.diagnosis-envelope/v1` has `confidence: unresolved`.
3. A confirmed bug diagnosis has `payload.affectedSurface: browser` and an evidence-backed repair direction.
4. A measured performance symptom is requested for diagnosis only; the dominant cost is external/environmental and the payload is non-actionable.

## Required observations

- Situation 1 terminates in `diagnosing-bugs` with an evidence-backed diagnosis; any reproducer or instrumentation remains handoff evidence rather than an integrated repair.
- Situation 2 makes `fix-bug` stop before the TDD/refactor path. The shared contract value is `unresolved`, not an invented `uncertain` confidence state.
- Situation 3 permits the red-to-green fix and then invokes `browser-investigation` through `diagnosis.payload.affectedSurface == browser` before repository verification.
- Situation 4 terminates in `diagnosing-performance` without invoking `optimize-performance` or disguising the external/environmental cause as an owned code refactor.
- Diagnostic capabilities may create bounded evidence artifacts without taking ownership of the integrated behavior/performance change.

## Forbidden behavior

- Start fixing situation 1 merely because a likely cause was found.
- Apply any behavior-changing repair from the unresolved diagnosis in situation 2.
- Read an undeclared top-level `diagnosis.affected-surface` field or skip required browser verification in situation 3.
- Optimize owned code in situation 4 despite the non-actionable external/environmental diagnosis.
- Continue diagnosis indefinitely after an acceptable terminal result has been reached.

## Acceptable outcomes

- Each diagnostic request stops with compact evidence at the diagnosis boundary.
- Remediation runs only when its required diagnosis state supports it, and preserves its own verification/review boundary.
- Missing human intent or unavailable external evidence is escalated explicitly rather than converted into a guessed repair.
