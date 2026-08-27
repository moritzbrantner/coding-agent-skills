# Coding Agent Skills

Canonical home for general, reusable coding-agent reasoning skills and declarative flows.

This repository is intentionally independent of `agent-loop-orchestrator`. Skills must remain usable in a direct human-to-agent session. Agent Loop may consume and wrap these capabilities with task, scope, authority, evidence, and runtime context, but general capabilities never depend on Agent Loop.

Ownership boundaries:

- `coding-agent-skills`: general reasoning skills, declarative flows, profiles, and source capability metadata.
- `coding-agent-conventions`: stable engineering policy and vocabulary.
- `coding-tooling`: deterministic parsing, validation, scope discovery, checks, and action implementations.
- `agent-contracts`: cross-repository interchange schemas.
- `agent-loop-orchestrator`: durable work state, scheduling, worktrees, retries, authority, and integration.
- `agent-loop-setup`: machine-level environment/bootstrap integration.

Capability sources live under `skills/*/SKILL.md` and `flows/*/FLOW.md`. Repository profiles live under `profiles/*.toml`. Generated catalog fragments are derived and are not committed.
