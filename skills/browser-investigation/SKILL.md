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
extensions: {}
---

# Browser Investigation

Use this capability when expected or observed behavior is browser-visible and a runnable application is available.

Prefer the repository's own development and test entrypoints. Use the installed Playwright CLI when available rather than inventing browser automation. Playwright's installed skills and CLI help are the command reference; this skill defines the engineering procedure and does not duplicate that manual.

1. Establish the smallest trustworthy local URL and state that exercise the behavior.
2. Use the caller-provided `PLAYWRIGHT_CLI_SESSION` when present; otherwise use one isolated named session for this investigation. Do not opt into a persistent browser profile unless the task genuinely requires state across browser restarts.
3. Inspect the accessibility snapshot first and interact through semantic element references or locators. Use coordinate or image interaction only for surfaces without an adequate semantic target.
4. Reproduce or exercise the path. Inspect console and network evidence when they can distinguish competing hypotheses; enable tracing for ambiguous or intermittent failures.
5. Prefer semantic state evidence over screenshots. Capture screenshots or video when visual layout or transient presentation is itself relevant.
6. Treat cookies, storage state, traces, HARs, screenshots, and videos as potentially sensitive. Do not commit authentication state or unsanitized captured session data.
7. For a behavior change or bug fix, convert the finding into the smallest durable repository-owned automated test at the appropriate browser boundary. Interactive exploration is evidence, not the regression gate.
8. Close the investigation session unless outer orchestration owns its lifecycle.

If the browser environment is unavailable, report that evidence boundary explicitly and continue only with the strongest repository-level evidence available; do not claim browser verification occurred.
