# Behavioral skill evaluations

This directory contains source-owned behavioral cases for reusable coding-agent capabilities.

The cases answer a different question from capability-source validation:

> If an agent follows this capability on a representative engineering problem, does it observe the right evidence, respect the right boundary, and stop in an acceptable state?

`coding-agent-skills` owns the scenarios and expected behavior. Deterministic parsing, case execution, result normalization, and aggregate reporting belong in `coding-tooling`; do not add a second evaluator here.

## Case shape

Each case is Markdown with stable frontmatter and these sections:

- `Task` — the instruction presented to the capability.
- `Given` — repository/runtime facts the case establishes.
- `Required observations` — evidence or distinctions the capability must surface.
- `Forbidden behavior` — actions or conclusions that make the run unacceptable.
- `Acceptable outcomes` — valid terminal states; a case may explicitly allow a no-change result.

Frontmatter fields:

- `id` — stable `eval/<name>` identifier.
- `capabilities` — capabilities exercised by the case.
- `critical` — whether violating a forbidden behavior blocks promotion of an affected provisional capability.

Cases describe semantics, not provider-specific prompts. They must not depend on a particular model, issue tracker, orchestrator, or hidden state.

## Evidence

A behavioral run should preserve enough evidence to determine which required observations were met, whether any forbidden behavior occurred, and which acceptable outcome was reached. A prose answer that merely contains expected keywords is not sufficient evidence of correct behavior.

These fixtures are intentionally small and adversarial. Add a case when a real failure reveals a reusable reasoning mistake, especially one that deterministic repository checks cannot decide on their own.
