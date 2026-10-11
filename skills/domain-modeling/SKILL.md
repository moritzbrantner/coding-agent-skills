---
id: "general/domain-modeling"
name: "domain-modeling"
description: "Elicit and maintain settled domain knowledge, terminology, and consequential decisions."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["domain", "architecture", "documentation"]
requires: []
related-to: ["general/grilling", "general/grill-with-docs", "general/codebase-design"]
readiness: []
extensions: {}
---

# Domain Modeling

This is both a direct entry point and a composable reasoning skill.

Inspect existing repository-owned Decisions/Questions (prefer scoped `daq` queries when a Decision Atlas and the tool are available), `CONTEXT.md`, domain docs, ADRs, code and tests before changing durable knowledge. Owner-approved current decisions determine intended product behavior; code/tests establish implementation evidence, not permission to change intent. Investigate genuine contradictions and ask one owner question if needed. In a repository without an Atlas, follow its established decision-document authority.

## Durable knowledge

- Keep `CONTEXT.md` concise: glossary, key concepts, and orientation only.
- Put richer knowledge in a configurable hierarchical domain tree, defaulting to `docs/domain/`.
- Keep the tree domain-first rather than mirroring folders/packages.
- Child domains inherit applicable parent knowledge. A child does not override parent truth. If a child exposes an overbroad parent claim, rewrite or qualify the parent at the correct generality.
- Humans may record stable, meaningful domain-to-code mappings. Tool-discovered mappings are useful evidence but are not domain truth and are not written back automatically.
- Write only settled knowledge. Do not persist provisional questions as facts.

When a domain decision becomes settled, record the owner's concise Decision (or Question) in the owning Atlas when present; otherwise use the repository's existing authoritative document. Supporting domain docs and ADRs may expand on it, but must not become a competing product-decision source. **Tell the user exactly what was written before continuing.**

## ADRs

Use ADRs for consequential architecture rationale when they add value; do not require one per Atlas entry. In an Atlas repository, revise intent at the owning stable ID with `~~retired wording~~ replacement` (or only strikethrough to retire), preserving Git history; existing explicit supersession links remain valid. Where ADRs are the repository's authoritative decision format, follow its established version/supersession convention. Avoid producing duplicate authorities.

Reuse the task’s [resolved policy context](../../docs/policy-context.md) for applicable engineering doctrine and repository-local exceptions. Use it for documentation paths and design vocabulary.
