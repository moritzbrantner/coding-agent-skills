---
id: "general/continue-next-slice"
name: "continue-next-slice"
description: "Continue repository work by selecting the single highest-priority deterministic next slice before doing new planning."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["continue", "next", "roadmap", "repository"]
requires: []
related-to: ["general/prepare-task-packet", "general/triage", "general/implement", "general/choose-workflow"]
readiness:
  - predicate: "tool-available"
    tool: "coding-tooling"
extensions:
  agent.procedure:
    schemaVersion: 1
    routing:
      useWhen: ["The request is effectively `continue` or `do the next slice` and does not name a new capability or bounded task."]
      doNotUseWhen: ["The user already named the specific task, PR, issue, or capability to work on.", "A previously selected slice is already prepared and only implementation or review remains."]
      mutates: true
      approvalBoundary: "none"
    termination:
      terminal: true
      doneWhen: ["Exactly one deterministic `selected` candidate is contextualized into one bounded slice, prepared as a task packet when implementation is required, and handed to the appropriate implementation or review capability."]
      stopWithoutChangeWhen: ["The deterministic next-slice resolver returns no selected candidate.", "A higher-priority candidate cannot be inspected because required evidence is unavailable."]
      escalateWhen: ["The selected candidate exposes unresolved product, domain, architecture, testing, or behavior intent that cannot be settled from repository evidence."]
      evidenceRequired: ["The `coding-tooling next` result, selected candidate source, current repository state, and any prepared task packet are preserved for the single selected slice."]
      outOfScope: ["Selecting or executing a second slice in the same invocation.", "Inventing a parallel priority score or durable queue.", "Substituting a more interesting adjacent task for the deterministic selection."]
    artifacts:
      consumes: ["repository-state", "deterministic-next-slice"]
      produces: ["next-slice-selection", "task-packet", "capability-handoff"]
---

# Continue Next Slice

Use this when the request is effectively “continue,” “do the next slice,” or otherwise asks the agent to advance existing repository work without naming a new capability.

1. Run `coding-tooling next --json` in the target repository before inventing new work.
2. Treat `unavailable` as a real blocker. If a higher-priority PR, issue, or capability inventory cannot be inspected, report that boundary instead of selecting lower-priority work from incomplete evidence.
3. Use exactly the returned `selected` candidate. The deterministic ordering prefers reconciliation and integration of existing PR work before roadmap items, issues, actionable source TODOs, and finally unplanned capability gaps.
4. Inspect the selected item's source and current repository state to turn it into one bounded implementation slice. Do not substitute a more interesting adjacent task because it appears easier or broader.
5. Invoke `general/prepare-task-packet` for implementation work so the selected slice gains an exact baseline, preservation constraints, out-of-scope boundaries, derived validation, and semantic review requirements.
6. Hand off to the appropriate implementation/review capability for that selected item. A PR-review candidate should remain integration/review work rather than being reinterpreted as a fresh feature task.
7. Stop after that handoff. A later invocation may resolve another deterministic next slice; this invocation does not recursively continue into a second selection.

`coding-tooling next` is the ordering authority for this procedure; this skill supplies contextual reasoning around the selected candidate. Do not create a durable queue, hidden priority score, or second roadmap index here.
