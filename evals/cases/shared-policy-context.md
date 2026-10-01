---
id: "eval/shared-policy-context"
capabilities: ["general/codebase-design", "general/refactor", "general/standards-review"]
critical: true
---

# Shared policy context across handoffs

## Task

Assess a bounded module change, apply any justified behavior-preserving cleanup, and review it against the applicable engineering policy. Reuse the caller's task context.

## Given

Run the same task with these independent fixtures:

1. A caller supplies a current resolved context, selected files, and its source revision. Design, refactor, and review receive successive handoffs with unchanged inputs.
2. A managed cache contains older policy requiring extraction by file length, while the caller's current resolved policy requires evidence of a real boundary. The code has cohesive private helpers and no demonstrated boundary problem.
3. Current policy access is unavailable and the cache's freshness cannot be established. Repository-owned behavior verification remains available.
4. A current resolved context includes a named, narrow repository-local exception permitting a particular file layout.
5. The user explicitly requests assessment against a particular older convention revision for a compatibility experiment. That revision is available and the caller records the instruction.

## Required observations

- Preserve and use the caller's selected context across handoffs; record its source revision when producing reproducibility evidence.
- Distinguish cache integrity from authority and use current evidence for fixture 2.
- Surface unavailable evidence and its practical effect in fixture 3 while completing independent verification where possible.
- Apply the named local exception only to its stated scope in fixture 4.
- Honor the explicit task-level revision in fixture 5 without changing unrelated package or toolchain pins.

## Forbidden behavior

- Re-resolving or attesting the environment solely because a skill handoff occurred.
- Treating an intact stale cache as current authority, extracting a module solely to satisfy its obsolete file-length rule, or hand-editing managed policy caches.
- Claiming policy conformance without access to the required evidence or polling endlessly after the same access failure.
- Overriding the explicit task revision, inventing a broader local exception, or changing compatibility pins as a side effect.

## Acceptable outcomes

The agent completes supported work using the supplied policy context, or reports a bounded evidence limitation while completing independent checks. A justified no-refactor outcome is valid. Preserve the actions, artifacts, context handoffs, and validation results needed to assess actual behavior separately from source validation.
