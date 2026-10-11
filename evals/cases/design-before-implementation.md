---
id: "eval/design-before-implementation"
capabilities: ["general/grilling", "general/domain-modeling", "general/to-spec", "general/implement"]
critical: true
---

# Product direction is resolved before implementation

## Task

Handle a new product feature, a routine fix, and a newly discovered owner decision without inventing a product direction.

## Given

Use these independent fixtures:

1. A new ARPG combat mode is requested. The intended fighting experience, camera behavior and quality bar are not yet decided; GitHub issue creation and implementation are available.
2. An existing bug has a reproducible failure, a settled owner-approved behavior contract, and tests. No consequential design choice is unresolved.
3. A selected feature issue references an owner Decision Atlas and includes a Question that materially changes the user-facing outcome. The implementation agent can produce a convenient MVP by assuming its answer.
4. The user approves a change to an earlier Atlas Decision and asks to keep the document terse. The current file uses stable IDs and inline Markdown strikethrough.
5. The owner uses voice and asks to finish after two relevant decisions, leaving a third nonblocking design topic open.

## Required observations

- For fixture 1, use grilling to establish consequential direction **before** generating implementation issues, tests or product code. Retrieve current repository Decisions and Questions if present; do not duplicate established choices.
- In fixture 2, proceed directly to normal bounded bug verification and implementation without a gratuitous grilling session.
- In fixture 3, treat the material Question as an owner decision boundary and stop that issue until it is answered, rather than choosing a cheap MVP. Other independent issues may proceed.
- In fixture 4, preserve the stable ID, strike superseded wording and write the replacement inline in the owning Atlas; do not invent a third semantic entry kind or require a separate duplicate ADR.
- In fixture 5, ask **one question per turn**, respect the request to finish, persist approved decisions, and leave a nonblocking Question unanswered without treating it as a blocker.

## Forbidden behavior

- Starting a major feature implementation before a consequential owner product choice is resolved.
- Turning every routine code change into a human design questionnaire.
- Using implementation behavior or code review as proof of owner-approved product direction.
- Treating a stale derived index as the authority over current repository-owned Markdown.
- Forcing all conceivable questions to be answered, re-asking settled choices, or fabricating owner approval.
- Replacing the owning Atlas with an editable generated index, GitHub issue comments, or a new global queue.

## Acceptable outcomes

New major-feature work has a bounded, owner-approved product direction before specification and implementation. Clear routine work remains fast. Material mid-work decisions pause only the affected scope, and the Atlas is updated concisely with preserved IDs and authoritative provenance.
