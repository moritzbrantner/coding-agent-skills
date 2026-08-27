# Coding Agent Skills

Canonical home for the general coding-agent skills and declarative flows approved for the coding-agent landscape.

The repository is provider-agnostic and independently usable. General capabilities never depend on `agent-loop-orchestrator`; Agent Loop may consume and wrap them with task identity, scope, authority, evidence, worktrees, and durable runtime state.

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

Removed from the initial catalog: `research`, `characterize-feature`, `grill-me`, general `handoff`, setup-as-a-skill, and peripheral helper skills.

## Source model

- `skills/<name>/SKILL.md` — reusable reasoning procedures.
- `flows/<name>/FLOW.md` — executable DAG compositions.
- `profiles/*.toml` — automatic-use allowlists.
- generated catalog fragments — derived by `coding-tooling`; never committed.

The strict interchange shape is defined by `agent-contracts`. Deterministic parsing, graph validation, profile resolution, and action mechanics are owned by `coding-tooling`.

## Validate

Run:

```bash
scripts/validate-capabilities
```

The command delegates to `coding-tooling`; this repository intentionally does not duplicate its parser/validator.

## Landscape boundaries

- `coding-agent-skills`: reasoning skills, flows, profiles, source capability metadata.
- `coding-agent-conventions`: stable engineering policy and vocabulary.
- `coding-tooling`: deterministic discovery, parsing, checks, scope resolution, and actions.
- `agent-contracts`: cross-component contracts.
- `agent-loop-orchestrator`: durable work state, scheduling, worktrees, retries, authority, candidates, receipts, and integration.
- `agent-loop-setup`: machine-level bootstrap/environment integration.
