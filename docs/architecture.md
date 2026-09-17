# Architecture

The capability layer is intentionally progressive.

A direct agent can invoke a skill or flow with no orchestrator. A larger runtime can wrap the same capability with task identity, write scope, authority, evidence, and durable execution state. The general capability itself does not know or care which outer layer exists.

## Skill graph

Capability frontmatter is the authoritative graph source. `requires` declares hard capability availability. Flow `invoke` steps declare executable edges. `related-to` is descriptive. Consumers may merge this repository's generated catalog fragment with fragments from agent-loop-specific repositories.

### Procedure metadata

Capabilities with meaningful routing ambiguity may declare versioned `extensions.agent.procedure` metadata. The neutral shape is `agent.capability-procedure-metadata/v1` from `agent-contracts`; deterministic validation is owned by `coding-tooling`.

The metadata records three things that should not depend on prompt phrasing:

- **routing** — when the capability is appropriate or inappropriate, whether it mutates repository state, and whether a human approval boundary exists;
- **termination** — observable done, no-change, escalation, evidence, and out-of-scope conditions so an invocation has a finite stopping rule;
- **artifacts** — stable semantic identities for the inputs it can consume and outputs it produces, without moving artifact persistence into this repository.

This metadata supplements the reasoning procedure; it does not replace the skill body or turn the catalog into a workflow engine. Add it where neighboring capabilities are easy to confuse or where an explicit termination/approval boundary materially reduces rework. Roll it out from dogfooded cases rather than mechanically stamping every capability with generic text.

## Flow model

Flows support sequence, nested parallel work, conditions, explicit human-confirmation gates, and typed deterministic actions. Required actions make a flow not-ready if unavailable. Optional actions must declare their fallback. Durable retries and scheduling stay outside the flow layer.

Structured flow outputs can be bound explicitly to later inputs, while ordinary conversational context remains available for incidental information.

## Documents and work artifacts

Canonical specs and tickets are human-readable Markdown. Deterministic tooling parses and validates them one-way into structured models. The structured projection is never independently editable.

Specifications have stable immutable IDs. Tickets have stable immutable IDs and bind to both the parent spec ID and the exact spec content revision/hash from which they were derived.

Under Agent Loop, ticket artifacts live in per-user orchestrator runtime storage. Standalone callers may keep the same Markdown ticket format in their own chosen queue location.

## Domain knowledge

`CONTEXT.md` is a concise glossary/overview. Rich domain knowledge defaults to a configurable `docs/domain/` tree. The hierarchy is domain-first, not code-folder-first. Child domains inherit applicable parent knowledge and do not override parent truth; an overbroad parent statement must be qualified at the correct level.

ADRs capture consequential decisions. Minor clarifications may edit an ADR; a real decision change creates a new ADR and supersedes the old one.
