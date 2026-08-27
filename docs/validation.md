# Validation

`coding-tooling` is the only deterministic parser/validator for this repository's capability sources. Local validation is:

```bash
scripts/validate-capabilities
```

A strict hosted CI gate should invoke the same `agent-capabilities validate` operation once the shared `coding-tooling` composite action exposes that operation directly. Do not copy the parser into this repository merely to make CI self-contained.

Validation must cover stable IDs, frontmatter shape, profile inheritance/resolution, stable entry-point profile membership, flow DAG cycles, required/optional deterministic action semantics, readiness predicates, and generated catalog construction.
