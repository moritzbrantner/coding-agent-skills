# Coding Agent Skills

Canonical home for the general coding-agent skills and declarative flows approved for the coding-agent landscape.

The repository is provider-agnostic and independently usable. General capabilities never depend on `agent-loop-orchestrator`; Agent Loop may consume and wrap them with task identity, scope, authority, evidence, worktrees, and durable runtime state.

## Responsibility

Skills answer:

> How should an agent perform this kind of work?

They own reusable reasoning procedures and flows: implementation, debugging, review, refactoring, planning, architecture work, and similar activities.

They do **not** own shared code policy. `coding-agent-conventions` answers what resulting code must satisfy. Repository-local `AGENTS.md` files own repository-specific context, commands, architecture boundaries, and exceptions.

## Initial stable catalog

`minimal` enables:

- `grilling`
- `grill-with-docs`
- `domain-modeling`
- `to-spec`
- `implement`
- `tdd`
- `refactor`
- `code-review`
- `standards-review`
- `spec-review`

`standard` extends `minimal` with:

- `to-tickets`
- `reconcile-tickets`
- `review-and-fix`
- `diagnosing-bugs`
- `fix-bug`
- `diagnosing-performance`
- `optimize-performance`
- `prototype`
- `codebase-design`
- `architecture-review`
- `improve-codebase-architecture`
- `intake-assessment`
- `triage`
- `conflict-resolution`
- `resolve-merge-conflicts`
- `choose-workflow`

## Provisional skills

Provisional skills are discoverable but are not enabled by the stable `minimal` or `standard` profiles until their procedure has been exercised and deliberately promoted.

- `capability-internalization` — evaluate whether an external capability should remain external, gain a stable boundary, or be replaced by a smaller specialized implementation using parity, performance, and real-consumer evidence.

Removed from the initial catalog: `research`, `characterize-feature`, `grill-me`, general `handoff`, setup-as-a-skill, and peripheral helper skills.

## Source model

- `skills/<name>/SKILL.md` — reusable reasoning procedures.
- `flows/<name>/FLOW.md` — executable DAG compositions.
- `profiles/*.toml` — automatic-use allowlists.
- generated catalog fragments — derived by `coding-tooling`; never committed.

The strict interchange shape is defined by `agent-contracts`. Deterministic parsing, graph validation, profile resolution, and action mechanics are owned by `coding-tooling`.

## Convention context

Skills do not copy engineering doctrine from `coding-agent-conventions`.

For repository work, prefer the repository's committed policy context:

1. read repository-local `AGENTS.md` guidance;
2. read `.conventions/index.md` and the relevant installed module snapshots when present;
3. use `coding-tooling conventions check` when convention-installation integrity matters;
4. apply repository-local instructions as the most specific policy.

Skills must not require live access to the shared conventions repository during ordinary work. `coding-tooling conventions resolve` is only a migration fallback for repositories that have not yet adopted installed convention modules.

A repository's `conventions.lock.json` records which shared policy revision was installed. That lock is repository evidence and update state; skills should not invent their own policy pinning or synchronization mechanism.

## Validate

Run:

```bash
scripts/validate-capabilities
```

The command delegates to `coding-tooling`; this repository intentionally does not duplicate its parser/validator.

## Landscape boundaries

- `coding-agent-skills`: reusable reasoning skills, flows, profiles, and source capability metadata.
- `coding-agent-conventions`: shared engineering policy and registry vocabulary.
- repository `AGENTS.md`: repository-specific context and exceptions.
- `coding-tooling`: deterministic discovery, parsing, checks, convention installation/integrity, and action mechanics.
- `agent-contracts`: cross-component contracts.
- `agent-loop-orchestrator`: optional durable work state, scheduling, worktrees, retries, authority, candidates, receipts, and integration.
- `agent-loop-setup`: machine-level bootstrap/environment integration.
