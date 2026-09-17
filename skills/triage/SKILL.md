---
id: "general/triage"
name: "triage"
description: "Classify an observed problem or work request using evidence, scope, severity, and next-needed reasoning without fixing it."
kind: "skill"
maturity: "stable"
entry-point: true
intents: ["triage", "classify", "issue"]
requires: []
related-to: ["general/intake-assessment", "general/diagnosing-bugs", "general/diagnosing-performance", "general/choose-workflow"]
readiness: []
extensions:
  agent.procedure:
    schemaVersion: 1
    routing:
      useWhen: ["An observed problem or work request needs evidence-based classification by scope, impact, reproducibility, and next-needed reasoning."]
      doNotUseWhen: ["The request is still too ambiguous to establish what is observed.", "A root cause is already established and the task is to implement the repair."]
      mutates: false
      approvalBoundary: "none"
    termination:
      terminal: true
      doneWhen: ["The observed condition is classified with scope, impact, confidence, and the smallest appropriate next capability or human decision."]
      stopWithoutChangeWhen: ["Available evidence does not establish a defect or actionable work item beyond recording the observed condition."]
      escalateWhen: ["Severity is material but evidence is insufficient to classify the condition safely.", "The next step depends on unresolved human intent rather than further technical evidence."]
      evidenceRequired: ["Observed behavior, affected scope, impact, reproducibility, and available repository or runtime evidence were distinguished from hypotheses."]
      outOfScope: ["Implementing a fix.", "Creating durable issue-tracker or queue state merely to classify work."]
    artifacts:
      consumes: ["request-context", "repository-state", "runtime-evidence"]
      produces: ["triage-result"]
---

# Triage

Triage is evidence-oriented classification, not implementation.

Establish what is actually observed, affected scope, user/system impact, reproducibility, urgency, and whether the problem is a bug, performance issue, architecture concern, dependency/update issue, ambiguous request, or operational condition.

Use repository/runtime evidence directly. Separate severity from confidence: an uncertain severe symptom is not a confirmed root cause. Identify the smallest next reasoning capability or human decision needed.

Issue trackers are optional publication/sync surfaces. Do not require an issue, add workflow labels, or create durable queue state merely to triage.

Stop after a clear classification and recommended next capability; do not silently begin the fix.
