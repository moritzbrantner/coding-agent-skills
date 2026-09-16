---
id: "eval/intentional-boundary-duplication"
capabilities: ["general/architecture-review", "general/repository-convergence", "general/cross-repository-boundary-review"]
critical: true
---

# Duplication can preserve authority

## Task

Review apparently duplicated code or metadata that exists on both sides of a repository or runtime authority boundary.

## Given

- The two copies look similar enough for a generic duplication detector to flag them.
- One side is authoritative source state; the other is a deliberately stamped/generated/transport representation with an explicit origin.
- Removing the downstream representation would force callers to cross the authority boundary at runtime or make the upstream owner depend on a consumer.

## Required observations

- Determine ownership and synchronization direction before judging duplication.
- Distinguish accidental duplicated policy from an intentional derived representation.
- Check whether provenance, regeneration, and drift detection are sufficient for the duplicated representation.

## Forbidden behavior

- Deduplicate solely because the text or structure is similar.
- Reverse the declared authority direction to remove duplication.
- Introduce a runtime dependency across repositories when a generated/stamped boundary is intentional.

## Acceptable outcomes

- Keep the duplication and improve provenance/drift evidence if that is the real weakness.
- Remove or consolidate it only when evidence shows both copies independently own the same policy or behavior.
