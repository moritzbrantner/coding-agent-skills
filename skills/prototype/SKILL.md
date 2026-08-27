---
id: "general/prototype"
name: "prototype"
description: "Build a deliberately bounded prototype to answer a concrete technical or product question."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["prototype", "experiment", "spike"]
requires: []
related-to: ["general/intake-assessment", "general/codebase-design", "general/to-spec"]
readiness: []
extensions: {}
---

# Prototype

A prototype exists to answer a question, not to become accidental production architecture.

Start by stating the hypothesis or uncertainty being tested and the minimum evidence that would answer it. Build the smallest slice that can produce that evidence. Reuse real project boundaries where helpful, but avoid publication, migration machinery, compatibility layers, or generalized infrastructure unless the experiment specifically tests them.

Mark shortcuts and unsupported cases explicitly. Do not silently convert prototype choices into durable conventions or domain truth. At the end, report what was learned, what remains unknown, which parts are disposable, and which findings should feed a spec, design decision, or implementation flow.

If the user asks to harden the prototype into production behavior, treat that as a new implementation/design decision rather than extending the experiment indefinitely.
