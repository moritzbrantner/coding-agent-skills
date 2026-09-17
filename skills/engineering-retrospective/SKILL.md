---
id: "general/engineering-retrospective"
name: "engineering-retrospective"
description: "Explain why an engineering problem survived as long as it did and route the smallest reusable prevention improvement to its owning layer."
kind: "skill"
maturity: "provisional"
entry-point: true
intents: ["retrospective", "root-cause", "prevention", "process"]
requires: []
related-to: ["general/repository-convergence", "general/architecture-review", "general/diagnosing-bugs", "general/diagnosing-performance", "general/standards-review"]
readiness: []
extensions:
  agent.procedure:
    schemaVersion: 1
    routing:
      useWhen: ["A completed or substantially understood engineering incident, repair, review cycle, or performance investigation needs a prevention-oriented retrospective across implementation, architecture, procedure, tooling, policy, observability, task decomposition, orchestration, or external infrastructure."]
      doNotUseWhen: ["The immediate problem is not diagnosed well enough to explain what happened.", "The task is simply to implement an already-selected prevention change."]
      mutates: false
      approvalBoundary: "none"
    termination:
      terminal: true
      doneWhen: ["The retrospective identifies the direct cause, why detection or correction was delayed, earlier signals that were missed or unavailable, evidence and confidence, and the smallest owning layer for each justified prevention improvement; it may explicitly conclude that no systemic change is warranted."]
      stopWithoutChangeWhen: ["The evidence supports a one-off local defect whose existing repair and tests are sufficient and no reusable prevention improvement is justified."]
      escalateWhen: ["The prevention choice depends on consequential architecture, policy, product, or domain intent that evidence cannot settle.", "The owner of the prevention improvement remains ambiguous after inspecting the relevant repository and landscape boundaries."]
      evidenceRequired: ["Relevant diagnosis, implementation or review history, CI/verification attempts, exact candidate evidence, traces or profiles when applicable, correlated agent-run Performance Evidence when available, and available task/handoff context are inspected; direct evidence is distinguished from inference."]
      outOfScope: ["Implementing the prevention changes.", "Creating durable backlog, queue, or run-history state.", "Inventing a convention or deterministic gate from a single non-generalizable incident.", "Reassigning ownership merely to make the retrospective produce an action item.", "Collapsing agent effort into a productivity score or ranking people, models, or providers from incomparable workloads."]
    artifacts:
      consumes: ["engineering-incident-evidence", "diagnosis-envelope", "review-findings", "verification-evidence", "performance-evidence"]
      produces: ["retrospective-findings", "prevention-routing"]
---

# Engineering Retrospective

Use this after an engineering problem is sufficiently understood to ask a different question from diagnosis:

> Why did this survive as long as it did, and what is the smallest reusable change that would let us prevent or detect this class of problem earlier next time?

This is a read-only feedback capability. It does not implement the prevention work, create backlog state, or turn every incident into a new rule.

## Establish the incident

Reconstruct only the evidence needed to explain the incident:

- the user/system-visible failure or engineering cost;
- the direct technical cause that was eventually established;
- the sequence of important implementation, diagnosis, review, CI, benchmark, or integration attempts;
- when decisive evidence first existed versus when it was actually used;
- assumptions, missing observability, misleading green checks, authority confusion, stale evidence, or task decomposition choices that materially delayed convergence.

Prefer exact commits, PR heads, check results, profiles, traces, diagnosis envelopes, review findings, task packets, handoff receipts, and canonical Performance Evidence when available. Do not infer a process failure merely because the repair was difficult.

When agent-run Performance Evidence is available, correlate records by the producer-owned run/attempt identity and exact source/candidate provenance rather than by timestamps or filenames. Useful evidence can include invocation/retry counts, execution time, deterministic time-to-green, CI/tool/wait spans, token categories, escalation stage, and whether a candidate was produced. Treat missing telemetry as unknown rather than zero.

## Separate cause from detection delay

Report at least these two layers independently:

1. **Direct cause** — the implementation, data model, architecture, environment, contract, or other mechanism that produced the defect or cost.
2. **Detection/prevention gap** — why the normal development process did not expose or prevent that cause earlier.

A local bug can have a systemic detection gap; an architectural performance problem can also have been caught promptly. Do not collapse these questions.

Identify the earliest realistic point where the class of problem could have been caught with information that was actually available then. A hindsight-only signal is not a prevention mechanism.

## Use agent-cost evidence without inventing a productivity metric

Agent timing, tokens, retries, CI wait, and escalation evidence can make a retrospective more precise, but these measurements are explanatory signals, not a single score.

Use correlated evidence to test concrete hypotheses such as:

- repeated local optimization attempts consumed substantial work before architecture/data-movement evidence was inspected;
- one CI lane dominated wait time while contributing no distinct failure signal;
- repeated retries were caused by stale or incomplete exact-head evidence rather than implementation difficulty;
- escalation to a stronger procedure or local execution loop happened later than the available evidence justified;
- architecture-oriented diagnosis cost more up front but materially reduced later retries under comparable workloads.

Compare agent cost only when workload identity, source/candidate provenance, environment, and relevant execution semantics are sufficiently comparable. Keep provider/model identity as context when it materially affects execution, not as a reason to rank models from unrelated tasks. Never infer human productivity or engineering quality from token count or elapsed time alone.

## Route prevention to the owning layer

Classify each justified prevention recommendation under the smallest layer that can own it coherently:

- **implementation** — a local coding repair, invariant, or focused regression test is sufficient;
- **architecture** — ownership, interfaces, data movement, dependency direction, lifetime, or topology must change or be reviewed earlier;
- **skill/procedure** — the reusable agent procedure needs a better reasoning step, routing distinction, evidence requirement, or termination rule;
- **deterministic check/tooling** — a mechanically decidable condition should be encoded in `coding-tooling` or another authoritative deterministic tool instead of prose;
- **convention/policy** — a genuinely reusable engineering rule belongs in `coding-agent-conventions`, with deterministic enforcement where practical;
- **observability** — missing measurements, traces, counters, profiles, or evidence packaging prevented timely diagnosis;
- **task decomposition** — scope, prerequisites, preservation constraints, or slice boundaries made the work harder to reason about or verify;
- **orchestration/runtime** — durable scheduling, retries, worktree ownership, coordination, receipts, or integration state caused the delay and belongs outside individual skills;
- **external/infrastructure** — the blocking condition belongs to a hosted service or environment outside the owned repositories; record the boundary instead of manufacturing a repository change.

Do not copy a recommendation into multiple layers merely to appear comprehensive. Pick the lowest coherent owner first; add another layer only when it addresses a distinct reusable failure mechanism.

For agent-landscape findings, preserve the same boundary discipline. A recurring late architecture escalation belongs in a skill/procedure; a mechanically detectable missing receipt belongs in tooling; run/attempt coordination belongs in the orchestrator; portable measurement semantics belong in `performance-evidence`; cross-component interchange belongs in `agent-contracts`; artifact preservation belongs in reusable workflows. Do not move semantics into a transport layer because the retrospective observed them there.

## Require generalization evidence

Before recommending a new shared convention, deterministic gate, procedure rule, or orchestration feature, state why the incident generalizes beyond the single repair. Useful evidence includes recurrence across repositories/tasks, a structurally repeatable failure mode, a high-cost invariant that is cheap to mechanize, or an existing class of near misses.

Correlated Performance Evidence can strengthen this generalization case when the same costly pattern recurs across comparable attempts. One expensive run is not enough to prove a landscape rule. Prefer several attributable examples, or a deterministic structural invariant, before changing shared procedure or policy.

If the incident is a one-off local defect and the repair plus focused regression evidence is sufficient, say so and stop. “No new systemic mechanism justified” is a successful retrospective outcome.

A heuristic finding, aesthetic preference, or retrospective intuition is not enough to create policy. Shared doctrine still belongs to `coding-agent-conventions`; deterministic mechanics still belong to `coding-tooling`; cross-component interchange still belongs to `agent-contracts`; durable coordination still belongs to an orchestrator/runtime.

## Output

Return a compact retrospective containing:

- incident and direct cause;
- detection/correction delay and missed earlier signals;
- evidence timeline with confidence and uncertainty;
- relevant correlated agent-run cost evidence when it materially explains the delay or prevention opportunity;
- prevention recommendations, each with owning layer and why that layer is authoritative;
- recommendations explicitly rejected as overreach, when relevant;
- whether the incident indicates a local-only repair or a reusable landscape improvement;
- the smallest next prevention slice, if one is justified.

Stop after the retrospective and prevention routing. A separate invocation implements any selected prevention change.
