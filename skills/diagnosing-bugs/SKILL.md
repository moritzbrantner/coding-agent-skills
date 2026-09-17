---
id: "general/diagnosing-bugs"
name: "diagnosing-bugs"
description: "Reproduce and isolate a bug, producing an evidence-backed diagnosis without integrating a fix."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["bug", "diagnose", "debug"]
requires: []
related-to: ["general/fix-bug", "general/tdd", "general/browser-investigation"]
readiness: []
extensions:
  agent.procedure:
    schemaVersion: 1
    routing:
      useWhen: ["An observed correctness problem needs reproduction, boundary isolation, and root-cause evidence before repair."]
      doNotUseWhen: ["A trusted diagnosis already identifies the cause and the task is to implement the repair.", "The primary problem is measured performance rather than incorrect behavior."]
      mutates: true
      approvalBoundary: "none"
    termination:
      terminal: true
      doneWhen: ["The diagnosis envelope records the symptom, reproducer, minimized case, evidence-backed cause hypothesis, confidence, and useful repair direction or explicitly records that the cause remains unresolved."]
      stopWithoutChangeWhen: ["Evidence is sufficient to diagnose the problem without adding a reproducer or instrumentation artifact.", "Evidence establishes that no owned product-code repair is justified by the observed symptom."]
      escalateWhen: ["Intended behavior or an external fact needed to distinguish competing diagnoses is genuinely unavailable."]
      evidenceRequired: ["Expected versus observed behavior, a trustworthy reproducer, the narrowed failing boundary, tested competing hypotheses, and explicit confidence are recorded."]
      outOfScope: ["Integrating the bug fix.", "Turning an unresolved technical hypothesis into a repair merely to make a symptom disappear."]
    artifacts:
      consumes: ["request-context", "repository-state", "runtime-evidence"]
      produces: ["diagnosis-envelope", "diagnostic-handoff-evidence"]
---

# Diagnosing Bugs

Diagnose before fixing.

Reproduce the symptom on the smallest trustworthy path, establish expected versus observed behavior, narrow the failing boundary, test competing hypotheses, and identify the root cause with explicit confidence and evidence.

For browser-visible symptoms, use `general/browser-investigation` when it is available rather than diagnosing only from source. Capture only the semantic browser, console, network, or trace evidence needed to isolate the failing boundary.

Produce the shared `agent.diagnosis-envelope/v1` bug payload from `agent-contracts`, including symptom, reproducer, minimized case, cause hypothesis, candidate repair directions when supported, top-level confidence, and evidence references. When the behavior requires post-fix verification in a real browser, set `payload.affectedSurface` to the normalized value `browser`; callers must read that value from the bug payload rather than inventing a separate top-level diagnosis field.

Use `confidence: unresolved` when the available evidence cannot establish a sufficiently trustworthy cause. An unresolved diagnosis is a valid terminal diagnosis result; callers must not treat it as permission to guess a repair.

You may create a failing regression test, reproducer, or instrumentation change as **handoff evidence**, but do not integrate the behavior fix and do not turn diagnosis into a hidden repair. `fix-bug` decides whether that evidence becomes permanent and owns the red-to-green transition.

Do not ask the human for logs, versions, repository facts, or runtime evidence you can obtain yourself. Ask only when intended behavior or an external fact is genuinely unavailable.

If evidence remains ambiguous, say so explicitly rather than selecting the most convenient hypothesis.
