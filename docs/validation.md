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

Hosted CI invokes the same tooling through an exact source-pinned `coding-tooling` composite action. It verifies deterministic conformance, validates capability sources, and records deterministic repository findings without depending on a floating publication or package release. Findings remain advisory evidence unless repository policy explicitly promotes them or independent review establishes a defect.

Validation covers stable IDs, strict frontmatter shape, profile inheritance/resolution, stable entry-point profile membership, flow DAG cycles, required/optional action semantics, readiness predicates, action references, and generated catalog construction.

Generated catalog fragments are validation output only and are not committed.
