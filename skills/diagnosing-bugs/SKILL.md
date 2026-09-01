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
extensions: {}
---

# Diagnosing Bugs

Diagnose before fixing.

Reproduce the symptom on the smallest trustworthy path, establish expected versus observed behavior, narrow the failing boundary, test competing hypotheses, and identify the root cause with explicit confidence and evidence.

For browser-visible symptoms, use `general/browser-investigation` when it is available rather than diagnosing only from source. Capture only the semantic browser, console, network, or trace evidence needed to isolate the failing boundary.

Produce the shared diagnosis envelope expected by `agent-contracts`, including symptom/reproduction evidence, likely root cause, confidence, affected surface, and useful next verification. Use the normalized affected-surface value `browser` when the behavior requires post-fix verification in a real browser; this lets executable caller flows schedule that verification deterministically.

You may create a failing regression test, reproducer, or instrumentation change as **handoff evidence**, but do not integrate it and do not turn diagnosis into a hidden fix. `fix-bug` decides whether that test becomes permanent and owns the red-to-green transition.

Do not ask the human for logs, versions, repository facts, or runtime evidence you can obtain yourself. Ask only when intended behavior or an external fact is genuinely unavailable.

If evidence remains ambiguous, say so explicitly rather than selecting the most convenient hypothesis.
