# Policy context for repository work

Engineering authority is defined by [REPO-018](https://github.com/moritzbrantner/coding-agent-conventions/blob/main/conventions/repository/README.md#repo-018--track-the-current-shared-convention-authority); repository-local exceptions follow [REPO-002](https://github.com/moritzbrantner/coding-agent-conventions/blob/main/conventions/repository/README.md#repo-002--more-specific-conventions-override-broader-conventions). Skills consume that authority rather than choosing a different revision policy.

At task entry, read repository-local instructions and module selection, then use the normal `coding-tooling conventions resolve` entry path to obtain the applicable context. Reuse a context already resolved by the caller, including its selected files, `sourceRevision`, local exceptions, and any explicit task-specific revision instruction. Read the relevant resolved files before making governed decisions. Pass that context through skill handoffs; a handoff alone does not require another resolution, cache repair, or environment attestation. Refresh through the normal tooling path when policy inputs change or validation needs current evidence.

If a managed cache is stale, missing, or corrupt, let deterministic tooling resolve or repair it. `conventions check` describes cache integrity, not current authority. Do not hand-edit managed snapshots or promote an older lock to authority merely because it passes an integrity check.

When current policy access is unavailable, report the missing evidence and its effect on the task. Continue independent work where useful, but do not label a stale cache current, claim conformance to unread policy, or repeatedly poll without new evidence. An explicit user instruction selecting a revision remains part of the task context; do not silently replace it. Package, ABI, toolchain, contract, and reproducible-input pins retain their own semantics.

When recording reproducibility evidence, use the resolved `sourceRevision`. Existing cache locks describe installation state and historical records remain historical; neither is a new consumer policy pin.
