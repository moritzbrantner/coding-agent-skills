# Validation

`coding-tooling` is the only deterministic parser/validator for this repository's capability sources.

Local capability validation is:

```bash
scripts/validate-capabilities
```

When `coding-tooling` is available, verify the installed convention snapshot independently with:

```bash
coding-tooling conventions check
```

`conventions.json`, `conventions.lock.json`, and `.conventions/` form the installed policy snapshot. Ordinary validation must not silently replace that committed snapshot with live policy.

`.coding-tooling.json` declares the intended repository-level `package:check` tier. Current `coding-tooling` component discovery only creates package, Rust, and .NET components, so executing that root-level tier in a Markdown/Shell repository is not yet a valid completion gate. That tooling gap is tracked as `coding-tooling#233`; do not add a fake language/package manifest merely to satisfy discovery.

Hosted CI checks out the exact pull-request head (or exact pushed revision), installs the pinned Bun runtime from its checksum-verified release artifact, fetches an accepted exact `coding-tooling` source revision, and invokes that revision's convention-integrity check, capability parser, and findings entrypoints directly. This keeps `coding-tooling` authoritative without depending on a floating publication.

The workflow verifies the fetched `coding-tooling` commit before execution and fails closed on runtime download/checksum failure, tooling revision drift, convention-snapshot drift, capability-validation failure, or deterministic-findings failure. Findings remain advisory evidence unless repository policy explicitly promotes them or independent review establishes a defect. Do not copy validator behavior into this repository or weaken failures to work around Actions infrastructure.

Capability validation covers stable IDs, strict frontmatter shape, profile inheritance/resolution, stable entry-point profile membership, flow DAG cycles, required/optional action semantics, readiness predicates, action references, and generated catalog construction. Convention validation covers the selected module set and every managed snapshot hash.

Generated catalog fragments and validation reports are output only and are not committed.
