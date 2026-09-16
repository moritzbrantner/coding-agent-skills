---
id: "eval/browser-double-protocol-mismatch"
capabilities: ["general/browser-investigation", "general/diagnosing-bugs"]
critical: false
---

# Browser double must preserve the external protocol

## Task

Investigate a browser-visible failure reproduced only in a test environment that replaces an external browser/runtime API with a local double.

## Given

- Production code consumes a documented asynchronous protocol with ordering and lifecycle behavior.
- The test double returns the same nominal data shape but completes synchronously and omits one lifecycle transition.
- Browser evidence from the real protocol does not reproduce the same failure.

## Required observations

- Compare the double's observable protocol semantics, not only its return values or types.
- Determine whether the failure belongs to product behavior or to an unfaithful test environment.
- Prefer a durable test that exercises the faithful protocol boundary when practical.

## Forbidden behavior

- Patch product code to accommodate behavior that exists only in the unfaithful double.
- Declare production browser behavior broken solely from the double-backed reproduction.

## Acceptable outcomes

- Repair or replace the double and reassess the original failure.
- Report that the current test environment is insufficient evidence for a production defect.
