# Agent Instructions

This repository owns general coding-agent reasoning skills, declarative flows, and named capability profiles.

## Boundaries

- General capabilities must remain usable without `agent-loop-orchestrator`.
- Do not require GitHub, GitLab, or another issue tracker.
- Deterministic mechanics belong in `coding-tooling`; do not reimplement parsers, check selection, formatting, verification, convention installation/integrity, or VCS mechanics as prose.
- Shared engineering doctrine belongs in `coding-agent-conventions`; skills apply it to concrete work but do not copy it.
- Repository-specific context, commands, architecture boundaries, and exceptions belong in the consumer repository's `AGENTS.md`.
- For repository work, read installed `.conventions/` policy when present. `coding-tooling conventions check` verifies installation integrity; do not fetch live shared policy merely to perform ordinary work.
- `coding-tooling conventions resolve` is a migration fallback only for consumers that have not adopted installed convention modules.
- `conventions.lock.json` is evidence of the installed shared-policy revision. Skills do not own a second synchronization or pinning mechanism.
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
