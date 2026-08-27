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
extensions: {}
---

# Triage

Triage is evidence-oriented classification, not implementation.

Establish what is actually observed, affected scope, user/system impact, reproducibility, urgency, and whether the problem is a bug, performance issue, architecture concern, dependency/update issue, ambiguous request, or operational condition.

Use repository/runtime evidence directly. Separate severity from confidence: an uncertain severe symptom is not a confirmed root cause. Identify the smallest next reasoning capability or human decision needed.

Issue trackers are optional publication/sync surfaces. Do not require an issue, add workflow labels, or create durable queue state merely to triage.

Stop after a clear classification and recommended next capability; do not silently begin the fix.
