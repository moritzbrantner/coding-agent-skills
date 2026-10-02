---
id: "eval/unblock-issues-asks-one-dependent-question"
capabilities: ["general/unblock-issues", "general/grilling"]
critical: true
---

# Unblock issues asks only the current decision-frontier question

## Task

Review the current blocked issues and help the human resolve the decisions that prevent further implementation.

## Given

- One blocked issue waits on an open technical prerequisite and needs no human input.
- One blocked issue waits only on a manual review action with no design choice.
- One blocked issue contains a genuine product decision with two downstream questions whose answers depend on that product choice.
- Repository and tracker evidence are sufficient to explain the product choice without asking the human to investigate facts.
- No actionable implementation issue remains in the caller's current scan.

## Required observations

- Inspect the blocked set once and classify the three blocker kinds correctly.
- Leave the open technical prerequisite as a dependency rather than converting it into a question.
- Surface the manual-review action directly rather than pretending it is a design decision.
- Resolve available facts before questioning the human.
- Ask exactly one currently answerable product question, with concise evidence and consequences.
- Do not ask either downstream question until the product choice is answered.
- After an answer, record the settled decision at the existing durable owner when appropriate and recompute the frontier from the same snapshot.

## Forbidden behavior

- Ask all three product questions in one batch.
- Ask the human for repository, CI, or linked-issue facts that tools can discover.
- Create a durable question queue, claim, retry counter, or new workflow state.
- Treat every blocked issue as requiring human judgment.
- Begin implementing newly unblocked work inside the unblock invocation.

## Acceptable outcomes

- The dependency remains linked, the manual action is surfaced, one product question is asked, downstream questions remain deferred, and no orchestration-only durable state is introduced.
