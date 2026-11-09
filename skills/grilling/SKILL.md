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

## Decision gate versus research alternatives

Before asking the owner to choose, classify the proposed options:

- **Exclusive decision:** incompatible product, architecture, behavioral or binding choices for the *next authorized action*, not already settled by evidence. Ask one focused question and record the answer.
- **Nonexclusive inquiry:** rival hypotheses, explanatory standards, semantic readings, alternative models or experiments that can be retained and tested concurrently. Preserve the defensible variants, strongest arguments, objections and replies, and their conditional consequences. Do not manufacture a winner question or turn the agent's recommendation into owner approval. Ask for a preference only if prioritization itself blocks progress.
- **Unknown relationship:** test overlap, coverage and exclusivity; do not assume that numbered options are exhaustive or pairwise incompatible. A user request to keep all variants is a settled methodological instruction, not an unanswered decision.

A recommendation may guide the next investigation without committing to the truth of a hypothesis. Reserve mandatory owner selection for a genuine choice whose alternatives cannot be pursued together in the requested outcome.

## Procedure

1. Inspect owner-approved intent and current repository evidence. Do not reopen settled decisions or invent product goals.
2. Build a small dependency-aware frontier of consequential unanswered questions for the intended product outcome. Resolve discoverable facts yourself and apply the decision-versus-inquiry classification above.
3. For genuine exclusive decision gates, ask **one material question per turn**, in prerequisite order; keep voice/driving turns short. Prefer genuinely different options with brief trade-offs; do not send batches or force UI choices when not relevant. For research alternatives, keep the branches and continue inquiry without demanding selection.
4. Incorporate the answer and continue only while more questions would materially change direction. Leave nonblocking unknowns open; stop when the owner wants to finish.
5. Persist owner-approved Decisions and unresolved Questions through the owning repository's documentation procedure. In an Atlas repository, answer Questions with linked Decisions and revise intent using its current inline `~~old~~ replacement` convention; preserve stable IDs. Never make a proposed inference authoritative.
6. If an earlier decision changes, revisit only the choices that truly depend on it.

The skill is complete when **sufficient direction for the current outcome** is settled or the owner stops—not when every imaginable question is exhausted. It does **not** add a final confirmation gate; the calling flow decides whether one is required.

Do not create durable task state, run a scheduler, or invent retries. In a direct session, use available tools for fact finding. Under an orchestrator wrapper, the wrapper may delegate fact finding without changing this reasoning procedure.
