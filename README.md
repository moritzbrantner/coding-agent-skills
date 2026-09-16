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
- `browser-investigation`
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
- `prepare-task-packet`
- `continue-next-slice`
- `prepare-handoff`
- `final-integration-review`
- `cross-repository-boundary-review`

## Provisional skills

Provisional skills are discoverable for explicit use but are not enabled by the stable `minimal` or `standard` automatic-use profiles until their procedure has been exercised and deliberately promoted.

- `capability-internalization` — evaluate whether an external capability should remain external, gain a stable boundary, or be replaced by a smaller specialized implementation using parity, performance, and real-consumer evidence.
- `repository-convergence` — improve one repository-owned seam at a time from a real deterministic or independently substantiated finding, then verify the exact candidate head without turning heuristic scores into policy.

Removed from the initial catalog: `research`, `characterize-feature`, `grill-me`, general `handoff`, setup-as-a-skill, and peripheral helper skills.

## Source model

- `skills/<name>/SKILL.md` — reusable reasoning procedures.
- `flows/<name>/FLOW.md` — executable DAG compositions.
- `profiles/*.toml` — automatic-use allowlists.
- `evals/cases/*.md` — source-owned behavioral scenarios and acceptance boundaries.
- `docs/promotion.md` — evidence required to move a provisional capability into automatic-use stability.
- generated catalog/evaluation reports — derived by tooling; never committed as independent source truth.

The strict interchange shape is defined by `agent-contracts`. Deterministic parsing, graph validation, profile resolution, evaluation execution, and action mechanics are owned by `coding-tooling`.

## Behavioral evaluation

Source validation proves that a capability is well-formed; it does not prove that the procedure makes good engineering decisions. Behavioral cases under `evals/cases/` exercise consequential distinctions such as exact-head evidence, advisory-vs-blocking findings, authority boundaries, valid no-change outcomes, and architectural performance costs.

Promotion from `provisional` to `stable` follows `docs/promotion.md`. Critical behavioral boundary violations block promotion even if other cases succeed; real-consumer dogfooding is also required.

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

When `coding-tooling` is available, also verify the installed policy snapshot with:

```bash
coding-tooling conventions check
```

This repository intentionally does not duplicate the `coding-tooling` parser/validator. `.coding-tooling.json` records the intended repository-level validation tier; executing root-level tiers in repositories without a language component is tracked upstream in `coding-tooling#233`.

## Landscape boundaries

- `coding-agent-skills`: reusable reasoning skills, flows, profiles, behavioral cases, and source capability metadata.
- `coding-agent-conventions`: shared engineering policy and registry vocabulary.
- repository `AGENTS.md`: repository-specific context and exceptions.
- `coding-tooling`: deterministic discovery, parsing, checks, evaluation execution, convention installation/integrity, and action mechanics.
- `agent-contracts`: cross-component contracts.
- `agent-loop-orchestrator`: optional durable work state, scheduling, worktrees, retries, authority, candidates, receipts, and integration.
- `agent-loop-setup`: machine-level bootstrap/environment integration.
