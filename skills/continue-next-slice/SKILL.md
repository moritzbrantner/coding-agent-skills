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
extensions: {}
---

# Continue Next Slice

Use this when the request is effectively “continue,” “do the next slice,” or otherwise asks the agent to advance existing repository work without naming a new capability.

1. Run `coding-tooling next --json` in the target repository before inventing new work.
2. Treat `unavailable` as a real blocker. If a higher-priority PR, issue, or capability inventory cannot be inspected, report that boundary instead of selecting lower-priority work from incomplete evidence.
3. Use exactly the returned `selected` candidate. The deterministic ordering prefers reconciliation and integration of existing PR work before roadmap items, issues, actionable source TODOs, and finally unplanned capability gaps.
4. Inspect the selected item's source and current repository state to turn it into one bounded implementation slice. Do not substitute a more interesting adjacent task because it appears easier or broader.
5. Invoke `general/prepare-task-packet` for implementation work so the selected slice gains an exact baseline, preservation constraints, out-of-scope boundaries, derived validation, and semantic review requirements.
6. Hand off to the appropriate implementation/review capability for that selected item. A PR-review candidate should remain integration/review work rather than being reinterpreted as a fresh feature task.

`coding-tooling next` is the ordering authority for this procedure; this skill supplies contextual reasoning around the selected candidate. Do not create a durable queue, hidden priority score, or second roadmap index here.
