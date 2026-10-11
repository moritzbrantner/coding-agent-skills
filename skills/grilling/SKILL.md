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

Use this skill **before writing implementation specs/issues for a new product or major feature** when the desired direction still has consequential owner choices. Use it during implementation only for genuine newly discovered forks. Already-specified work, routine bug fixes and behavior-preserving refactors should proceed without another grilling round.

Read the owning repository's current product decisions first. If it has a Decision Atlas, prefer scoped `daq` queries when available; otherwise use the relevant Markdown. Do not require GitHub, a particular Atlas format, or a separate orchestration tool.

## Procedure

1. Inspect owner-approved intent and current repository evidence. Do not reopen settled decisions or invent product goals.
2. Build a small dependency-aware frontier of consequential unanswered questions for the intended product outcome. Resolve discoverable facts yourself.
3. Ask **one material question per turn**, in prerequisite order; keep voice/driving turns short. Prefer genuinely different options with brief trade-offs; do not send batches or force UI choices when not relevant.
4. Incorporate the answer and continue only while more questions would materially change direction. Leave nonblocking unknowns open; stop when the owner wants to finish.
5. Persist owner-approved Decisions and unresolved Questions through the owning repository's documentation procedure. In an Atlas repository, answer Questions with linked Decisions and revise intent using its current inline `~~old~~ replacement` convention; preserve stable IDs. Never make a proposed inference authoritative.
6. If an earlier decision changes, revisit only the choices that truly depend on it.

The skill is complete when **sufficient direction for the current outcome** is settled or the owner stops—not when every imaginable question is exhausted. It does **not** add a final confirmation gate; the calling flow decides whether one is required.

Do not create durable task state, run a scheduler, or invent retries. In a direct session, use available tools for fact finding. Under an orchestrator wrapper, the wrapper may delegate fact finding without changing this reasoning procedure.
