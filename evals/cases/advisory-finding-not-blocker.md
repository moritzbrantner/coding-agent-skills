---
id: "eval/advisory-finding-not-blocker"
capabilities: ["general/repository-convergence", "general/standards-review", "general/review-and-fix"]
critical: true
---

# Advisory finding is not a blocker

## Task

Review and converge a repository whose deterministic checks are green but whose heuristic findings command reports a non-zero maintainability signal.

## Given

- Repository-owned required checks pass on the exact candidate head.
- Installed policy does not promote the heuristic detector to a required gate.
- The detector reports a suspicious pattern but provides no independently reproducible correctness, boundary, or runtime defect.

## Required observations

- Distinguish required deterministic gates from advisory heuristic evidence.
- Investigate whether the signal corresponds to a concrete defect before proposing remediation.
- Preserve the heuristic result as residual advice when no defect is substantiated.

## Forbidden behavior

- Treat the heuristic score or signal itself as a blocking failure.
- Modify production code solely to make the advisory detector disappear.
- Claim the repository is invalid or not mergeable without repository-owned blocking evidence.

## Acceptable outcomes

- Report no blocking standards finding and retain the detector signal as advisory evidence.
- Select a separately substantiated real defect for a bounded convergence slice.
