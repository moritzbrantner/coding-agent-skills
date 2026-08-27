---
id: "general/choose-workflow"
name: "choose-workflow"
description: "Advisory router that recommends the best available capability or flow and waits for human confirmation before acting."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["route", "workflow", "help"]
requires: []
related-to: ["general/intake-assessment", "general/grilling", "general/implement", "general/triage"]
readiness: []
extensions: {}
---

# Choose Workflow

Use the generated capability catalog plus the current request, readiness information, and any intake assessment to recommend the smallest appropriate capability/flow.

The router is advisory first:

1. Recommend one capability or a short flow and explain why.
2. Mention material readiness/degraded conditions.
3. It may recommend an installed-but-disabled capability when that is clearly better.
4. Wait for explicit human confirmation before invoking or continuing into the recommendation.

A confirmed one-off use of an installed-but-disabled capability does not modify the repository profile.

Do not turn the graph into a rigid workflow engine. Declared relationships are authoritative metadata, but routing remains contextual. Do not create tasks, durable state, retries, or an ontology of work.

When the chosen capability is confirmed, hand off to the caller/runtime for invocation with the current conversational context.
