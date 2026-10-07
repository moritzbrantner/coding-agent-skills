# Coding Agent Skills

Canonical home for the general coding-agent skills and declarative flows approved for the coding-agent landscape.

The repository is provider-agnostic and independently usable. Capabilities do not depend on an orchestrator or a particular work loop; the caller owns task selection, worktrees and integration.

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
- `conflict-resolution`
- `resolve-merge-conflicts`
- `final-integration-review`
- `cross-repository-boundary-review`

## Provisional skills

Provisional skills are discoverable for explicit use but are not enabled by the stable `minimal` or `standard` automatic-use profiles until their procedure has been exercised and deliberately promoted.

- `capability-internalization` — evaluate whether an external capability should remain external, gain a stable boundary, or be replaced by a smaller specialized implementation using parity, performance, and real-consumer evidence.
- `repository-convergence` — improve one repository-owned seam at a time from a real deterministic or independently substantiated finding, then verify the exact candidate head without turning heuristic scores into policy.
- `engineering-retrospective` — explain why an engineering problem survived as long as it did and route the smallest reusable prevention improvement to implementation, architecture, procedure, tooling, policy, observability, task decomposition, orchestration, or external infrastructure without manufacturing systemic work from every incident.
- `repository-conventions` — select and apply an explicit `coding-agent-conventions` module set from repository evidence, asking the owner only about genuine policy choices and supporting one-question-at-a-time voice interaction.

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

`coding-agent-skills` owns those scenarios and expected behavior. `coding-tooling` owns deterministic case parsing/execution/reporting; do not add a second provider-specific evaluator here.

See [`evals/README.md`](evals/README.md) for case shape and [`docs/promotion.md`](docs/promotion.md) for maturity criteria.

## Convention context

Skills do not copy engineering doctrine from `coding-agent-conventions`.

Use the shared [policy-context procedure](docs/policy-context.md) for repository work. Reuse the caller’s resolved context across skill handoffs; convention authority remains owned by REPO-018 and local precedence by REPO-002.

## Validate

Run:

```bash
scripts/validate-capabilities
```

When `coding-tooling` is available, also check managed convention-cache integrity with:

```bash
coding-tooling conventions check
```

This repository intentionally does not duplicate the `coding-tooling` parser/validator. `.coding-tooling.json` records the intended repository-level validation tier; executing root-level tiers in repositories without a language component is tracked upstream in `coding-tooling#233`.

## Landscape boundaries

- `coding-agent-skills`: reusable reasoning skills, flows, profiles, behavioral cases, and source capability metadata.
- `coding-agent-conventions`: shared engineering policy and registry vocabulary.
- repository `AGENTS.md`: repository-specific context and exceptions.
- `coding-tooling`: deterministic discovery, parsing, checks, convention installation/integrity, behavioral-evaluation execution, and action mechanics.
- `agent-contracts`: cross-component contracts.
- the caller (a direct agent session, such as the global work loop in `moritzbrantner/dotfiles`): task selection, worktrees and integration, with GitHub issues as the durable work state.
