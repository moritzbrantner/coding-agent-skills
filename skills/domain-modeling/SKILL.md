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

Inspect existing `CONTEXT.md`, domain docs, ADRs, code, tests, and relevant evidence before changing durable knowledge. When code, tests, ADRs, and domain documentation disagree, no source wins automatically: investigate the contradiction, explain it, and ask the human which behavior is intended.

## Durable knowledge

- Keep `CONTEXT.md` concise: glossary, key concepts, and orientation only.
- Put richer knowledge in a configurable hierarchical domain tree, defaulting to `docs/domain/`.
- Keep the tree domain-first rather than mirroring folders/packages.
- Child domains inherit applicable parent knowledge. A child does not override parent truth. If a child exposes an overbroad parent claim, rewrite or qualify the parent at the correct generality.
- Humans may record stable, meaningful domain-to-code mappings. Tool-discovered mappings are useful evidence but are not domain truth and are not written back automatically.
- Write only settled knowledge. Do not persist provisional questions as facts.

When a domain decision becomes settled, write the appropriate durable change and **immediately tell the user exactly what was written before continuing**. If the user asks to rewrite the last change, edit it and then resume.

## ADRs

Recommend an ADR when a decision is hard to reverse, surprising, or involves a real tradeoff, but ask before creating one. Default ADRs are minimal: context, decision, and why. Add alternatives, consequences, status, or other sections only when useful. A typo or clarification may edit an ADR; an actual decision change creates a new ADR and marks the previous one superseded.

Use repository-local guidance and installed `.conventions/` modules for documentation paths and design vocabulary when present. The repository's committed policy context is authoritative subject to local overrides. If `conventions.json` or `conventions.lock.json` exists but the managed snapshots are missing or corrupt, report that failure rather than replacing them with live policy. Use `coding-tooling conventions resolve` only for repositories that have not yet adopted installed convention modules.
