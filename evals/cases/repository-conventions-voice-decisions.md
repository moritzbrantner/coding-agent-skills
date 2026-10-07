---
id: "eval/repository-conventions-voice-decisions"
capabilities: ["general/repository-conventions"]
critical: true
---

# Repository convention selection through focused voice decisions

## Task

Set the coding-agent convention modules for the repository. The owner is interacting by voice and may be driving.

## Given

Run the capability against these independent fixtures:

1. A TypeScript React Vite application already uses Vitest and Playwright. The repository is not yet initialized for shared conventions.
2. A Rust physics library already selects `rust` and contains benchmarks, but repository guidance does not establish whether runtime cost is an explicit engineering contract.
3. A repository currently selects an optional module whose associated technology has been removed, but no owner decision to weaken policy is recorded.
4. A repository has a valid `conventions.json`, a stale or damaged managed cache, and current deterministic convention tooling is available.
5. A voice-first session has two consequential choices where the second depends on the first.

## Required observations

- Discover technologies, repository role, current module selection, and local instructions from repository evidence instead of asking the owner for those facts.
- Treat registry dependency closure and managed-cache mechanics as deterministic concerns rather than human choices.
- In fixture 1, form an evidence-backed explicit selection from the observed stack without asking whether React, Vite, Vitest, or Playwright are used.
- In fixture 2, distinguish "Rust with benchmarks" from "performance is an explicit contract" and ask one concise consequential question before selecting the stronger performance policy.
- In fixture 3, do not silently remove the existing module; surface policy weakening as an owner decision unless prior intent already settles it.
- In fixture 4, use `coding-tooling` to repair/rematerialize and check the managed cache rather than editing generated policy files.
- In fixture 5, ask exactly one decision at a time, resolve the prerequisite first, and avoid requiring visual inspection of code, diffs, tables, or long lists.

## Forbidden behavior

- Asking the owner for repository facts that inspection can establish.
- Dumping the full convention catalog and asking the owner to choose modules without filtering it against repository evidence.
- Treating a registry profile as inherently superior to an explicit module set.
- Presenting pseudo-options where one option simply contains all capabilities of the others without a material cost or constraint.
- Silently weakening existing policy, inventing modules, copying shared convention text into `AGENTS.md`, or hand-editing `.conventions/` or `conventions.lock.json`.
- Claiming that `conventions check` establishes current policy freshness.

## Acceptable outcomes

The capability produces and applies an explicit evidence-backed module selection, with any genuine discretionary policy choices resolved through focused owner questions, and reports deterministic validation. A bounded stop is acceptable when a consequential owner decision or required deterministic tooling is unavailable.
