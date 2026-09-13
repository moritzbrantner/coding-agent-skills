---
id: "general/final-integration-review"
name: "final-integration-review"
description: "Perform the final exact-head semantic and mechanical review of a pull request and hand an approved candidate to existing integration mechanics."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["review", "integrate", "merge", "pull-request"]
requires: []
related-to: ["general/prepare-handoff", "general/review-and-fix", "general/code-review", "general/cross-repository-boundary-review"]
readiness: []
extensions: {}
---

# Final Integration Review

Use this when a pull request is believed to be complete and the remaining question is whether the exact candidate may be integrated.

1. Resolve the current PR head SHA and base SHA. Treat both as immutable inputs to this review.
2. Generate the mechanical receipt with those exact identities:

   `coding-tooling pr receipt <number> --expected-head <head-sha> --expected-base <base-sha> --json`

3. Stop unless the receipt passes. Required skipped, pending, failed, or unavailable checks are not green. Draft state, unmergeability, unresolved review threads, an unintegrated stack dependency, a moved head, or unavailable exact-head check evidence remains blocking.
4. Read the repository `AGENTS.md`, installed conventions, task packet/handoff when available, and the actual diff. Resolve every semantic review requirement against evidence. Pay particular attention to:
   - the repository's declared authority owner and prohibited write-back direction;
   - `mustPreserve` and `outOfScope` constraints;
   - public/API/README/Pages claims that must not exceed demonstrated capability;
   - compatibility, persistence, protocol, security, deterministic replay, and browser/mobile boundaries implicated by the change;
   - performance claims, which require equivalent benchmark evidence rather than merely a green functional suite.
5. Re-check review comments after repairs. Do not treat an outdated thread as resolved merely because its line moved; verify the concern no longer applies, reply with the repair evidence, and resolve the thread only then.
6. If the PR head changes at any point, discard the integration receipt and repeat the exact-head review.
7. When both the mechanical receipt and semantic review pass, return an integration-ready decision bound to the exact head SHA. If the caller already has explicit authority to integrate, hand that exact candidate to the repository's existing integration/auto-merge capability without asking for duplicate confirmation. Otherwise return the decision to the caller for the required authority step.

Do not implement merge queues, VCS mutation, check collection, or durable approval state in this skill. Those mechanics remain owned by `coding-tooling`, the hosting platform, or the caller/orchestrator.
