# Validation

`coding-tooling` is the only deterministic parser/validator for this repository's capability sources.

Canonical repository validation is:

```bash
coding-tooling run --tier default
```

The tier is declared in `.coding-tooling.json` and delegates `package:check` to:

```bash
scripts/validate-capabilities
```

`conventions.json`, `conventions.lock.json`, and `.conventions/` form the installed policy snapshot. Use `coding-tooling conventions check` to verify its integrity; ordinary validation must not silently replace it with live policy.

Hosted CI checks out the exact pull-request head (or exact pushed revision), installs the pinned Bun runtime from its checksum-verified release artifact, fetches an accepted exact `coding-tooling` source revision, and invokes that revision's canonical conformance, capability parser, and findings entrypoints directly. This keeps `coding-tooling` authoritative without depending on GitHub's cross-private-repository composite-action preparation path or a floating publication.

The workflow verifies the fetched `coding-tooling` commit before execution and fails closed on runtime download/checksum failure, tooling revision drift, deterministic-conformance failure, capability-validation failure, or deterministic-findings failure. Findings remain advisory evidence unless repository policy explicitly promotes them or independent review establishes a defect. Do not copy validator behavior into this repository or weaken failures to work around Actions infrastructure.

Validation covers installed-policy integrity and deterministic conformance plus stable IDs, strict frontmatter shape, profile inheritance/resolution, stable entry-point profile membership, flow DAG cycles, required/optional action semantics, readiness predicates, action references, and generated catalog construction.

Generated catalog fragments and validation reports are output only and are not committed.
