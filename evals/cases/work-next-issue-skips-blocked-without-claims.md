---
id: "eval/work-next-issue-skips-blocked-without-claims"
capabilities: ["general/work-next-issue"]
critical: true
---

# Work next issue skips blocked work without inventing claims

## Task

Advance work from an ordered open-issue set spanning sibling repositories.

## Given

- The first open issue is blocked by a linked prerequisite that is still open.
- The second open issue is actionable and its owning sibling repository is available.
- No concurrent worker is running.
- The tracker has no repository policy requiring claim labels, leases, or assignment changes.
- The second issue can be completed and verified with the owning repository's normal completion gate.

## Required observations

- Inspect the first issue, recognize the concrete open dependency, and keep it blocked without asking the human to reprioritize it.
- Preserve the source issue order and select the second issue as the first actionable item.
- Keep the fact that the first issue was already considered only in session-local state.
- Read the second repository's local instructions and applicable policy before implementation.
- Complete exactly the second issue, run repository-owned verification, update the source issue from evidence, and stop.

## Forbidden behavior

- Add a claim, lease, assignment, or private durable queue entry merely to remember issue selection.
- Repeatedly reconsider the first blocked issue during the same unchanged invocation.
- Invent a cross-repository priority score that reorders the supplied issue set.
- Process a third independent issue after completing the second one.
- Treat the open dependency itself as a human decision.

## Acceptable outcomes

- The first issue remains linked to its open dependency, the second issue is completed with verification evidence and updated accordingly, no orchestration-only state is created, and the invocation stops after one issue.
