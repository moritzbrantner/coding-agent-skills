---
id: "general/repository-conventions"
name: "repository-conventions"
description: "Select and apply coding-agent convention modules for a repository from repository evidence and focused owner decisions."
kind: "skill"
maturity: "provisional"
entry-point: true
intents: ["conventions", "policy", "setup"]
requires: []
related-to: ["general/grilling", "general/standards-review"]
readiness: []
extensions: {}
---

# Repository Conventions

Use this skill when a repository needs an initial `coding-agent-conventions` module selection or an intentional revision of its existing selection.

This skill owns the reasoning procedure for choosing repository policy. It does not own convention definitions, registry mechanics, cache materialization, or integrity checking.

## Procedure

1. Inspect the repository before asking questions:
   - read the applicable `AGENTS.md` files and other repository-local instructions;
   - read `conventions.json`, `conventions.lock.json`, and the managed `.conventions/` index when present;
   - inspect manifests, source roots, CI, tool configuration, and other direct evidence of languages, frameworks, infrastructure, and repository purpose.
2. Reuse caller-provided resolved policy context, including selected files, `sourceRevision`, and repository-local exceptions, for governed decisions. Obtain only missing catalog evidence through the normal policy/tooling path; honor an explicit task-selected revision and do not substitute a remembered catalog. Refresh the resolution after changing the module selection or when validation requires it.
3. Build the candidate module set from repository evidence:
   - treat dependency closure as deterministic, not as a human decision;
   - distinguish modules that directly match observed technologies or repository roles from modules that introduce an optional engineering contract;
   - retain an existing requested module unless there is evidence that its applicability should be reconsidered;
   - never invent a module or copy convention text into repository-local guidance.
4. Compare the current selection, the evidence-backed candidates, and any relevant registry profiles. A profile is a convenience composition, not automatically preferable to an explicit module set.
5. Resolve only the genuinely discretionary frontier with the owner. Ask about a choice only when repository evidence cannot determine whether the corresponding policy should apply. Explain the material consequence of each viable choice and make a recommendation when the evidence supports one.
6. Apply the final explicit selection using deterministic `coding-tooling` convention mechanics:
   - use `conventions init` for an uninitialized repository;
   - use `conventions add` for a purely additive change;
   - for a deliberate removal or replacement, change only the requested module selection in `conventions.json`, then use `conventions update` to rematerialize the managed cache;
   - when the selection is unchanged but the managed cache is stale or damaged, use `conventions update` to rematerialize it before the integrity check;
   - never hand-edit `conventions.lock.json` or files under `.conventions/`.
7. Run `coding-tooling conventions check --json`. Then run any repository validation required by the repository-local instructions for the changed source/configuration surface.
8. Report the requested modules, material owner decisions, local exceptions, and validation result. Do not claim that cache integrity proves current policy freshness.

## Human decisions

Do not ask the owner for discoverable facts such as whether the repository uses React, Rust, Vite, Postgres, Playwright, or a particular package when repository inspection can answer that.

A technology being present is evidence for the corresponding candidate module. A stronger optional contract may still need an owner decision. For example, benchmarks existing in a Rust repository do not by themselves establish that execution cost is an explicit engineering contract.

Do not silently remove existing policy merely because current evidence is incomplete. Treat policy weakening as an explicit decision unless the owner has already established the intended module set.

Use the dependency-aware questioning behavior from `grilling`: settle prerequisite choices before downstream ones and reopen only decisions affected by a revision.

## Voice / driving mode

When the caller indicates that interaction is voice-first or that the owner is driving:

- ask at most one unresolved consequential question per turn;
- keep the question and options short enough to answer with a letter or a brief phrase;
- use two or three options only when they represent genuine tradeoffs; do not manufacture a "general solution" that trivially dominates the others;
- state the practical consequence of each option verbally without requiring the owner to inspect code, diffs, paths, tables, or long module lists;
- resolve repository facts yourself between questions and continue immediately after each answer;
- do not add a final confirmation step when every consequential choice is already settled.

If no human decision remains, apply the evidence-backed explicit selection without forcing a questionnaire.
