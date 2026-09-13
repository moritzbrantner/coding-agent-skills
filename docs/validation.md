# Validation

`coding-tooling` is the only deterministic parser/validator for this repository's capability sources. Local validation is:

```bash
scripts/validate-capabilities
```

Hosted CI checks out the exact pull-request head (or the exact pushed revision), installs the pinned Bun runtime from its checksum-verified release artifact, fetches an accepted exact `coding-tooling` source revision, and invokes that revision's canonical capability parser and findings entrypoint directly. This keeps `coding-tooling` authoritative without depending on GitHub's cross-private-repository composite-action preparation path.

The workflow verifies the fetched `coding-tooling` commit before execution and fails closed on runtime download/checksum failure, tooling revision drift, capability-validation failure, or deterministic-findings failure. Do not copy validator behavior into this repository or weaken failures to work around Actions infrastructure.

Validation covers stable IDs, strict frontmatter shape, profile inheritance/resolution, stable entry-point profile membership, flow DAG cycles, required/optional action semantics, readiness predicates, action references, and generated catalog construction.

Generated catalog fragments are validation output only and are not committed.
