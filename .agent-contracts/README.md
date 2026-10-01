# Installed contract snapshot

This directory is a read-only consumer snapshot for repository-local hosted validation. It is **not** the authority for shared contract semantics; `agent-contracts` remains authoritative.

The snapshot records the exact source revision in `CATALOG.json`. Set `AGENT_CONTRACTS_ROOT` explicitly to use an exact source-development checkout. Hosted CI uses this committed snapshot because the authoritative repository is private and ordinary repository-local CI must not require cross-repository credentials.

Update these bytes only as an explicit contract-consumer change, together with the recorded source revision and the flow behavior that depends on the contract.
