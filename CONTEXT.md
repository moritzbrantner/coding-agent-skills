# Coding Agent Skills Context

`coding-agent-skills` is the canonical source repository for general reusable coding-agent capabilities.

## Vocabulary

- **skill** — reusable reasoning procedure.
- **flow** — declarative executable composition of skills and deterministic actions.
- **profile** — automatic-use allowlist for stable entry-point capabilities.
- **readiness** — capability-specific prerequisites; one unavailable capability must not block unrelated work.
- **general namespace** — stable capability IDs owned by this repository, such as `general/grilling`.
- **caller** — a human/direct agent session, Agent Loop wrapper, or another runtime that invokes a capability.
- **settled decision** — a human decision or evidence-backed conclusion that may be written to durable project documentation.
- **derived artifact** — deterministic projection such as a parsed spec/ticket model or generated capability catalog; never a second editable source of truth.

## Core ownership rule

General skills never depend on Agent Loop. Agent-loop-specific wrappers may depend inward on these capabilities and map their results to task packets, evidence, receipts, or runtime state.

`coding-agent-conventions` is authoritative for installed engineering policy. Skills contain minimal fallback defaults only so they remain independently usable.
