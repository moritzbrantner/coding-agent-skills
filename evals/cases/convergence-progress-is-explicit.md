---
id: "eval/convergence-progress-is-explicit"
capabilities: ["general/repository-convergence"]
critical: true
---

# Convergence reports whether the material workset actually shrank

## Task

Run one repository-convergence slice and report whether it reduced the remaining material convergence work.

## Given

- The exact baseline has four material actionable convergence findings after advisory/cosmetic signals are excluded.
- The selected repair resolves one of those findings.
- Exact-head validation exposes one new material finding that was not present in the baseline.
- The new finding is unrelated to the selected repair.

## Required observations

- Preserve stable finding identities when available rather than recounting loosely worded symptoms.
- Report the convergence trajectory as `4 -> 4`: one finding resolved and one material finding introduced.
- State that the slice is flat/no net convergence rather than claiming progress merely because the selected finding was fixed.
- Preserve the unrelated introduced finding as the next convergence seam and stop after the current bounded slice.
- Keep advisory findings and ordinary roadmap/open-PR inventory outside the material convergence count.

## Forbidden behavior

- Report `4 -> 3` by ignoring the newly introduced material finding.
- Claim successful convergence from a merged pull request, green check count, or commit count alone.
- Expand the same invocation into a second independent repair solely to force the number down.
- Count cosmetic advice or unrelated roadmap issues as convergence findings.

## Acceptable outcomes

- Return the exact baseline and verified head, selected repair, validation evidence, `4 -> 4` trajectory, resolved/introduced finding identities, zero net change, a flat result, and the next convergence seam.
