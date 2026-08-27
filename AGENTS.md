# Agent Instructions

This repository owns general coding-agent reasoning skills, declarative flows, and named capability profiles.

## Boundaries

- General capabilities must remain usable without `agent-loop-orchestrator`.
- Do not require GitHub, GitLab, or another issue tracker.
- Deterministic mechanics belong in `coding-tooling`; do not reimplement parsers, check selection, scope resolution, formatting, verification, convention resolution, or VCS mechanics as prose.
- Stable engineering doctrine belongs in `coding-agent-conventions`; skills apply it to concrete work. When shared policy is available, obtain the current applicable stack through `coding-tooling conventions resolve` rather than copied convention text or stale hard-coded paths. When conventions are unavailable, use only the minimal fallback behavior stated by the skill.
- Treat the convention resolver's `sourceRevision` as evidence about the policy applied to a run. Do not require consumer repositories to pin that revision merely to receive shared policy updates.
- Cross-repository structured interchange belongs in `agent-contracts`.
- Durable queues, scheduling, worktrees, retries, authority, run history, integration, and receipts belong in an orchestrator or caller, not in these skills.

## Capability sources

Each skill is `skills/<name>/SKILL.md`. Each flow is `flows/<name>/FLOW.md`. Every capability has a stable `general/<name>` ID and strict frontmatter accepted by `coding-tooling agent-capabilities`.

Flows are executable DAGs. They may contain capability invocations, typed deterministic actions, conditions, parallel steps, and explicit human gates. They must not contain arbitrary loops, durable retries, schedulers, or hidden state machines. Finite remediation is explicitly unrolled.

Generated catalog fragments are never committed. `coding-tooling` derives them from capability sources and `profiles/*.toml`.

## Human decisions

Do not ask the user for facts that can be discovered from the repository, tools, or available sources. Important product, domain, architecture, testing, conflict, or behavior choices remain human decisions when evidence does not settle intent.

## Profiles

`minimal` and `standard` are automatic-use allowlists, not hard availability boundaries. A caller may explicitly invoke another installed capability once without rewriting repository profile configuration.

Everything in the initial catalog is stable. Add provisional capabilities only deliberately.