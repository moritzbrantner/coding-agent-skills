# Validation

`coding-tooling` is the only deterministic parser/validator for this repository's capability sources. Local validation is:

```bash
scripts/validate-capabilities
```

Hosted CI invokes the same canonical parser through the version-pinned `coding-tooling` composite action. The workflow pins the exact source revision rather than depending on a floating publication or package release.

Validation covers stable IDs, strict frontmatter shape, profile inheritance/resolution, stable entry-point profile membership, flow DAG cycles, required/optional deterministic action semantics, readiness predicates, action references, and generated catalog construction.

Generated catalog fragments are validation output only and are not committed.
