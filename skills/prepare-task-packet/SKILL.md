---
id: "general/prepare-task-packet"
name: "prepare-task-packet"
description: "Turn one approved implementation slice into a bounded exact-baseline task packet with preservation, scope, risk, and review constraints."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["prepare", "task", "scope", "implement"]
requires: []
related-to: ["general/intake-assessment", "general/implement", "general/continue-next-slice", "general/prepare-handoff"]
readiness:
  - predicate: "tool-available"
    tool: "coding-tooling"
extensions:
  agent.procedure:
    schemaVersion: 1
    routing:
      useWhen: ["One implementation slice has already been selected or approved and needs an exact-baseline, machine-validated execution packet for another run or later continuation."]
      doNotUseWhen: ["The next slice has not yet been selected.", "The task is still ambiguous enough that scope or intended behavior must be resolved first."]
      mutates: true
      approvalBoundary: "none"
    termination:
      terminal: true
      doneWhen: ["Exactly one bounded task packet is written to ignored artifact storage and validates successfully against the exact baseline, preservation constraints, out-of-scope boundaries, derived capabilities, and semantic review requirements."]
      stopWithoutChangeWhen: ["The exact baseline cannot be established.", "Required task facts are missing and cannot be discovered without guessing."]
      escalateWhen: ["A necessary preservation, scope, architecture, testing, or behavior decision depends on unresolved human intent."]
      evidenceRequired: ["The selected slice, immutable baseline SHA, repository policy, preservation constraints, out-of-scope work, risk classification, and validation result are available."]
      outOfScope: ["Selecting a different next slice.", "Implementing the packet.", "Committing the packet or turning it into durable queue state."]
    artifacts:
      consumes: ["approved-implementation-slice", "repository-state", "installed-policy"]
      produces: ["task-packet"]
---

# Prepare Task Packet

Use this before substantial repository mutation when another agent, run, or later continuation must be able to recover the exact intent without reconstructing it from prose history.

1. Read the repository's `AGENTS.md` and installed `.conventions/` policy when present. Resolve the exact Git baseline SHA; never use `HEAD`, a branch name, or an inferred moving ref in the packet.
2. State one bounded `goal` and one `ownedCapability`. Use the repository's declared authority vocabulary when available.
3. Record `mustPreserve` from explicit user requirements, existing contracts, authority boundaries, and verified behavior. Do not invent preservation constraints merely to make the packet look complete.
4. Record `outOfScope` for adjacent work that must not be pulled into the slice.
5. Classify the changed risk using the task-packet `changeKinds`. Let `coding-tooling` derive deterministic capability requirements from that classification rather than copying a validation matrix into this skill.
6. Put semantic conditions that still require reasoning—authority preservation, claim scope, architectural intent, equivalent behavior, or other non-mechanical judgments—under `acceptance.reviewRequirements`. Never label them machine evidence.
7. Add explicit `acceptance.requiredCapabilities` only when the repository genuinely requires more than the derived set.
8. Write the ephemeral packet to an ignored path such as `.artifacts/coding-tooling/task.json` using schema `coding-tooling/task-packet/v1`.
9. Validate it with `coding-tooling agent task-packet .artifacts/coding-tooling/task.json --json`. Stop on `failed`, `error`, or `unavailable`; do not repair missing task facts by guessing.

The packet is execution evidence, not a durable backlog item. Do not commit it, mirror it into an issue merely for persistence, or grow it into orchestration state.
