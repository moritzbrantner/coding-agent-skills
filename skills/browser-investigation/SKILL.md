---
id: "general/browser-investigation"
name: "browser-investigation"
description: "Exercise browser-visible behavior through a real browser, collecting compact semantic evidence and turning durable findings into repository tests."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["browser", "web", "ui", "investigate"]
requires: []
related-to: ["general/diagnosing-bugs", "general/fix-bug", "general/implement", "general/code-review"]
readiness: []
extensions:
  agent.procedure:
    schemaVersion: 1
    routing:
      useWhen: ["Expected or observed behavior materially depends on real browser semantics, or a resolved diagnosis explicitly requires browser-boundary verification."]
      doNotUseWhen: ["The behavior can be established completely below the browser boundary and browser execution would add no material evidence."]
      mutates: true
      approvalBoundary: "none"
    termination:
      terminal: true
      doneWhen: ["The smallest trustworthy browser path has been exercised, material semantic evidence has been captured, durable regression evidence has been created when appropriate, and the investigation session is closed unless an outer caller owns it."]
      stopWithoutChangeWhen: ["Browser evidence disproves the product-code hypothesis or establishes that the observed failure is caused by a non-faithful test double or environment boundary.", "The requested browser verification cannot run and the limitation is reported without pretending verification occurred."]
      escalateWhen: ["Expected browser behavior depends on unresolved human intent.", "Required authentication or sensitive state cannot be obtained within the caller's authority."]
      evidenceRequired: ["The browser-visible state and only the relevant semantic, console, network, or trace evidence needed to support the conclusion are recorded."]
      outOfScope: ["Integrating an unrelated product fix merely because a browser symptom is visible.", "Treating screenshots alone as a durable regression gate when semantic automation is available."]
    artifacts:
      consumes: ["request-context", "repository-state", "diagnosis-envelope"]
      produces: ["browser-evidence", "regression-test-evidence"]
---

# Browser Investigation

Use this capability when expected or observed behavior is browser-visible and a runnable application is available.

Prefer the repository's own development and test entrypoints. Use the installed Playwright CLI when available rather than inventing browser automation. Playwright's installed skills and CLI help are the command reference; this skill defines the engineering procedure and does not duplicate that manual.

1. Establish the smallest trustworthy local URL and state that exercise the behavior.
2. Use the caller-provided `PLAYWRIGHT_CLI_SESSION` when present; otherwise use one isolated named session for this investigation. Do not opt into a persistent browser profile unless the task genuinely requires state across browser restarts.
3. Inspect the accessibility snapshot first and interact through semantic element references or locators. Use coordinate or image interaction only for surfaces without an adequate semantic target.
4. Reproduce or exercise the path. Inspect console and network evidence when they can distinguish competing hypotheses; enable tracing for ambiguous or intermittent failures.
5. When evidence crosses a mocked or stubbed network boundary, verify the double preserves the production protocol semantics before changing product code. Compare the relevant request method, status, headers, body, and stateful or streaming behavior such as redirects, cookies/authentication, CORS/cache handling, byte ranges and `206 Partial Content`, downloads, SSE, or WebSockets. Prefer the real local service when that is cheap and deterministic; otherwise use a protocol-faithful double. Treat a mismatch as a test-environment defect until the product hypothesis is independently established.
6. Prefer semantic state evidence over screenshots. Capture screenshots or video when visual layout or transient presentation is itself relevant.
7. Treat cookies, storage state, traces, HARs, screenshots, and videos as potentially sensitive. Do not commit authentication state or unsanitized captured session data.
8. For a behavior change or bug fix, convert the finding into the smallest durable repository-owned automated test at the appropriate browser boundary. Interactive exploration is evidence, not the regression gate.
9. Close the investigation session unless outer orchestration owns its lifecycle.

If the browser environment is unavailable, report that evidence boundary explicitly and continue only with the strongest repository-level evidence available; do not claim browser verification occurred.
