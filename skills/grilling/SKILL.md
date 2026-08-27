---
id: "general/grilling"
name: "grilling"
description: "Resolve a decision frontier through dependency-aware human questioning."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["clarify", "decide", "requirements"]
requires: []
related-to: ["general/grill-with-docs", "general/domain-modeling", "general/to-spec"]
readiness: []
extensions: {}
---

# Grilling

Use this skill when material human decisions are still unresolved.

## Procedure

1. Build a decision tree: identify open decisions and the prerequisite decisions each depends on.
2. Form the current frontier from only questions whose prerequisites are already settled.
3. Resolve discoverable facts yourself with repository inspection and available tools. Never ask the human for a fact you can obtain.
4. Ask the frontier in manageable adaptive batches. A large frontier may be split into multiple rounds, but never ask a downstream question early.
5. Incorporate each answer, update dependencies, and continue until the frontier is empty.
6. If the user revises an earlier decision, reopen only the decisions that actually depend on it.

The skill is complete when the frontier is empty. It does **not** add a final confirmation gate by itself; the calling flow decides whether one is required.

Do not create durable task state, run a scheduler, or invent retries. In a direct session, use available tools for fact finding. Under an orchestrator wrapper, the wrapper may delegate fact finding without changing this reasoning procedure.
