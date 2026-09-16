# Capability promotion

Capability maturity is an evidence claim, not a synonym for syntactic validity.

`stable` means the procedure is suitable for automatic-use profiles in its declared intent area. `provisional` means the capability is available for explicit use while its behavior is still being exercised and refined.

## Promotion evidence

A provisional capability may be promoted only when all of the following are true:

1. its source passes deterministic capability validation;
2. all applicable critical cases under `evals/cases/` have acceptable outcomes and no forbidden behavior;
3. non-critical case failures are either repaired or explicitly documented as a bounded limitation;
4. the procedure has been dogfooded in multiple materially different real repository/task contexts, with evidence of the input, important decisions, terminal state, and any human intervention;
5. observed failures have been turned into either a procedure correction, a behavioral eval, a deterministic check owned by `coding-tooling`, or an explicit out-of-scope boundary;
6. the capability's ownership does not duplicate policy from `coding-agent-conventions`, interchange contracts from `agent-contracts`, deterministic mechanics from `coding-tooling`, or durable state from an orchestrator.

Promotion does not require a universal numeric score. A single critical boundary violation is more important than many superficial successes. Evidence should show that the capability makes the consequential distinctions its procedure promises to make.

## Promotion record

A promotion change should summarize:

- behavioral cases exercised and their results;
- real-consumer contexts used for dogfooding;
- failures discovered while provisional and the changes they caused;
- known limitations that remain outside the declared capability;
- why automatic invocation is now safer than leaving the capability explicit-only.

Do not promote merely because a capability has existed for a certain amount of time or because its prose appears complete.
