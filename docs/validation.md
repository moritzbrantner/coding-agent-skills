# Validation

`coding-tooling` is the only deterministic parser/validator for this repository's capability sources.

Use the Bun version declared in `.bun-version` and an installed `coding-tooling`, or an explicit accepted source checkout with frozen dependencies:

```bash
export CODING_TOOLING_ROOT=/path/to/coding-tooling
bun install --cwd "$CODING_TOOLING_ROOT" --frozen-lockfile
bun "$CODING_TOOLING_ROOT/src/cli.ts" run --tier default --strict --json
bun "$CODING_TOOLING_ROOT/src/cli.ts" conformance --json
```

The default tier runs native ShellCheck and capability-source validation. The explicit source override takes precedence over an installed executable and fails if that checkout is unavailable. The focused catalog command remains:

```bash
scripts/validate-capabilities
```

When `coding-tooling` is available, check managed convention-cache integrity independently with:

```bash
coding-tooling conventions check
```

`conventions.json`, `conventions.lock.json`, and `.conventions/` describe module selection and managed cache state. Integrity evidence does not establish current policy authority; use the task’s [resolved policy context](policy-context.md) for implementation and review.

`.coding-tooling.json` declares root-level `lint` and `package:check` capabilities. Accepted tooling discovers them as an honest repository component, so this Markdown/Shell repository needs no fake language/package manifest. The former discovery gap (`coding-tooling#233`) is resolved. Strict conformance also checks portable text, actionable TODOs, path casing, symlink boundaries, and cache integrity. Environment-v1 adoption is optional and may remain advisory; it is not a missing validation capability.

Hosted CI installs the natively declared Bun runtime through the maintained setup action, fetches an accepted `coding-tooling` source revision with frozen dependencies, and runs the same default tier and conformance command as local work. Its findings remain a separate evidence surface. This keeps deterministic mechanics in `coding-tooling`; normal GitHub checks are the hosted merge evidence.

The workflow verifies the fetched `coding-tooling` commit before execution and fails closed on runtime setup failure, tooling revision drift, convention-snapshot drift, capability-validation failure, or deterministic-findings failure. Findings remain advisory evidence unless repository policy explicitly promotes them or independent review establishes a defect. Do not copy validator behavior into this repository or weaken failures to work around Actions infrastructure.

Capability validation covers stable IDs, strict frontmatter shape, profile inheritance/resolution, stable entry-point profile membership, flow DAG cycles, required/optional action semantics, readiness predicates, action references, and generated catalog construction. Convention validation covers the selected module set and every managed snapshot hash.

Generated catalog fragments and validation reports are output only and are not committed.
