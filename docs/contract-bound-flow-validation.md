# Contract-bound flow validation

A flow may bind a named `invoke` output to a neutral shared contract with `output-contract`. `coding-tooling` then validates downstream branch field paths and scalar comparison values against the referenced JSON Schema.

This mechanism exists for structured interchange that genuinely crosses an independently owned contract boundary. It is not a reason to assign schemas to ordinary conversational context or every intermediate flow value.

For diagnosis flows in this repository, `agent.diagnosis-envelope/v1` is authoritative in `agent-contracts`. The committed `.agent-contracts/` directory is a read-only consumer snapshot used only so hosted CI can remain repository-local without credentials for the private authoritative repository. Its `CATALOG.json` records the exact source revision. Local source-development validation should prefer an exact `AGENT_CONTRACTS_ROOT` or sibling checkout.

When the authoritative contract changes, update the flow, installed snapshot, recorded source revision, and validation evidence together. Never edit snapshot semantics independently to make a flow pass.
