---
id: "general/optimize-performance"
name: "optimize-performance"
description: "Measure a performance bottleneck, route the change by measured cause, verify correctness, remeasure, and review."
kind: "flow"
maturity: "stable"
entry-point: true
intents: ["performance", "optimize", "profile"]
requires: []
related-to: ["general/diagnosing-performance", "general/refactor", "general/architecture-review", "general/codebase-design", "general/code-review"]
readiness:
  - predicate: "action-available"
    action: "repository.verify"
flow:
  steps:
    - id: baseline-diagnosis
      kind: invoke
      capability: "general/diagnosing-performance"
      output: "baseline"
    - id: actionable
      kind: branch
      condition:
        source: "baseline.payload.actionable"
        equals: true
      then:
        - id: architecture-cause
          kind: branch
          condition:
            source: "baseline.payload.causeCategory"
            equals: "architecture-data-movement"
          then:
            - id: architecture-review
              kind: invoke
              capability: "general/architecture-review"
              inputs:
                performance-diagnosis: "baseline"
              output: "architecture-findings"
            - id: architecture-design
              kind: invoke
              capability: "general/codebase-design"
              inputs:
                performance-diagnosis: "baseline"
                findings: "architecture-findings"
              output: "proposed-design"
            - id: approve-architecture-change
              kind: human-gate
              prompt: "Approve the consequential ownership/interface change justified by the measured performance diagnosis before implementation."
            - id: optimize-architecture
              kind: invoke
              capability: "general/refactor"
              inputs:
                performance-diagnosis: "baseline"
                approved-design: "proposed-design"
              output: "optimized-change"
            - id: verify-architecture
              kind: action
              action: "repository.verify"
            - id: remeasure-architecture
              kind: invoke
              capability: "general/diagnosing-performance"
              inputs:
                comparison-baseline: "baseline"
              output: "after"
            - id: review-architecture
              kind: invoke
              capability: "general/code-review"
              output: "review-findings"
          else:
            - id: algorithm-cause
              kind: branch
              condition:
                source: "baseline.payload.causeCategory"
                equals: "algorithm-data-model"
              then:
                - id: algorithm-design
                  kind: invoke
                  capability: "general/codebase-design"
                  inputs:
                    performance-diagnosis: "baseline"
                  output: "proposed-design"
                - id: optimize-algorithm
                  kind: invoke
                  capability: "general/refactor"
                  inputs:
                    performance-diagnosis: "baseline"
                    design: "proposed-design"
                  output: "optimized-change"
                - id: verify-algorithm
                  kind: action
                  action: "repository.verify"
                - id: remeasure-algorithm
                  kind: invoke
                  capability: "general/diagnosing-performance"
                  inputs:
                    comparison-baseline: "baseline"
                  output: "after"
                - id: review-algorithm
                  kind: invoke
                  capability: "general/code-review"
                  output: "review-findings"
              else:
                - id: local-cause
                  kind: branch
                  condition:
                    source: "baseline.payload.causeCategory"
                    equals: "local-implementation"
                  then:
                    - id: optimize-local
                      kind: invoke
                      capability: "general/refactor"
                      inputs:
                        performance-diagnosis: "baseline"
                      output: "optimized-change"
                    - id: verify-local
                      kind: action
                      action: "repository.verify"
                    - id: remeasure-local
                      kind: invoke
                      capability: "general/diagnosing-performance"
                      inputs:
                        comparison-baseline: "baseline"
                      output: "after"
                    - id: review-local
                      kind: invoke
                      capability: "general/code-review"
                      output: "review-findings"
                  else:
                    - id: invalid-actionable-category
                      kind: human-gate
                      prompt: "The diagnosis is marked actionable but does not identify a supported owned code cause category. Stop without optimizing and correct the diagnosis or hand the external/environment issue to its owning caller."
      else:
        - id: insufficient-evidence
          kind: human-gate
          prompt: "The measured evidence does not justify a specific owned code optimization. Stop, or provide revised constraints/workload for one more diagnosis."
        - id: retry-decision
          kind: branch
          condition:
            source: "performance-decision.action"
            equals: "retry"
          then:
            - id: revised-diagnosis
              kind: invoke
              capability: "general/diagnosing-performance"
              output: "revised-baseline"
            - id: revised-actionable
              kind: branch
              condition:
                source: "revised-baseline.payload.actionable"
                equals: true
              then:
                - id: revised-architecture-cause
                  kind: branch
                  condition:
                    source: "revised-baseline.payload.causeCategory"
                    equals: "architecture-data-movement"
                  then:
                    - id: revised-architecture-review
                      kind: invoke
                      capability: "general/architecture-review"
                      inputs:
                        performance-diagnosis: "revised-baseline"
                      output: "revised-architecture-findings"
                    - id: revised-architecture-design
                      kind: invoke
                      capability: "general/codebase-design"
                      inputs:
                        performance-diagnosis: "revised-baseline"
                        findings: "revised-architecture-findings"
                      output: "revised-proposed-design"
                    - id: revised-approve-architecture-change
                      kind: human-gate
                      prompt: "Approve the consequential ownership/interface change justified by the revised measured diagnosis before implementation."
                    - id: revised-optimize-architecture
                      kind: invoke
                      capability: "general/refactor"
                      inputs:
                        performance-diagnosis: "revised-baseline"
                        approved-design: "revised-proposed-design"
                      output: "revised-optimized-change"
                    - id: revised-verify-architecture
                      kind: action
                      action: "repository.verify"
                    - id: revised-remeasure-architecture
                      kind: invoke
                      capability: "general/diagnosing-performance"
                      inputs:
                        comparison-baseline: "revised-baseline"
                      output: "revised-after"
                    - id: revised-review-architecture
                      kind: invoke
                      capability: "general/code-review"
                      output: "revised-review-findings"
                  else:
                    - id: revised-algorithm-cause
                      kind: branch
                      condition:
                        source: "revised-baseline.payload.causeCategory"
                        equals: "algorithm-data-model"
                      then:
                        - id: revised-algorithm-design
                          kind: invoke
                          capability: "general/codebase-design"
                          inputs:
                            performance-diagnosis: "revised-baseline"
                          output: "revised-proposed-design"
                        - id: revised-optimize-algorithm
                          kind: invoke
                          capability: "general/refactor"
                          inputs:
                            performance-diagnosis: "revised-baseline"
                            design: "revised-proposed-design"
                          output: "revised-optimized-change"
                        - id: revised-verify-algorithm
                          kind: action
                          action: "repository.verify"
                        - id: revised-remeasure-algorithm
                          kind: invoke
                          capability: "general/diagnosing-performance"
                          inputs:
                            comparison-baseline: "revised-baseline"
                          output: "revised-after"
                        - id: revised-review-algorithm
                          kind: invoke
                          capability: "general/code-review"
                          output: "revised-review-findings"
                      else:
                        - id: revised-local-cause
                          kind: branch
                          condition:
                            source: "revised-baseline.payload.causeCategory"
                            equals: "local-implementation"
                          then:
                            - id: revised-optimize-local
                              kind: invoke
                              capability: "general/refactor"
                              inputs:
                                performance-diagnosis: "revised-baseline"
                              output: "revised-optimized-change"
                            - id: revised-verify-local
                              kind: action
                              action: "repository.verify"
                            - id: revised-remeasure-local
                              kind: invoke
                              capability: "general/diagnosing-performance"
                              inputs:
                                comparison-baseline: "revised-baseline"
                              output: "revised-after"
                            - id: revised-review-local
                              kind: invoke
                              capability: "general/code-review"
                              output: "revised-review-findings"
                          else:
                            - id: revised-invalid-actionable-category
                              kind: human-gate
                              prompt: "The revised diagnosis is marked actionable but does not identify a supported owned code cause category. Stop without optimizing and correct the diagnosis or hand the external/environment issue to its owning caller."
              else:
                - id: still-insufficient
                  kind: human-gate
                  prompt: "The second diagnosis is still not actionable. Stop without optimizing or take the problem back to a new outer investigation."
          else: []
extensions:
  agent.procedure:
    schemaVersion: 1
    routing:
      useWhen: ["A performance problem needs an owned optimization path from measured diagnosis through implementation, correctness verification, identical-scenario remeasurement, and review."]
      doNotUseWhen: ["The request asks only for diagnosis or baseline evidence without changing code.", "The dominant cause is known to be external/environmental and no owned code optimization is justified."]
      mutates: true
      approvalBoundary: "conditional"
    termination:
      terminal: true
      doneWhen: ["An actionable owned-code diagnosis is routed to the correct local, algorithm/data-model, or architecture path and the resulting candidate is verified, remeasured against the same scenario, and reviewed; or the bounded non-actionable path stops without optimization."]
      stopWithoutChangeWhen: ["The diagnosis is non-actionable and the human chooses not to revise the workload or constraints.", "The second bounded diagnosis remains non-actionable.", "The diagnosis points to an external/environmental or unsupported cause rather than an owned code optimization."]
      escalateWhen: ["A consequential architecture/data-movement optimization requires explicit human approval before mutation.", "The representative workload or acceptable performance/correctness tradeoff depends on unresolved human intent.", "Verification or final review leaves blocking findings after the bounded optimization pass."]
      evidenceRequired: ["The before baseline, diagnosis routing metadata, repository correctness verification, identical-scenario after measurement, and independent review evidence are preserved for any applied optimization."]
      outOfScope: ["Claiming success without remeasurement.", "Unbounded retries after a second non-actionable diagnosis.", "Treating external/environmental costs as local code refactors."]
    artifacts:
      consumes: ["request-context", "repository-state", "runtime-evidence"]
      produces: ["performance-diagnosis", "optimized-change", "verification-evidence", "performance-comparison", "review-findings"]
---

# Optimize Performance

Optimization is evidence-driven. `diagnosing-performance` must first produce the measured `agent.diagnosis-envelope/v1` performance payload, including the routing metadata defined by the compatible `agent-contracts` revision used by this flow.

An actionable diagnosis is routed by its measured dominant cause rather than treating every performance problem as a local refactor:

- `local-implementation` goes directly to a bounded behavior-preserving refactor;
- `algorithm-data-model` goes through `codebase-design` before the bounded implementation so representation/complexity choices are explicit;
- `architecture-data-movement` goes through `architecture-review`, then `codebase-design`, then a mandatory human approval gate before the consequential ownership/interface refactor;
- `external-environment` and `unresolved` are not owned code-optimization paths for this flow and must not silently fall through to local refactoring.

The change must preserve intended behavior unless a human explicitly approves a tradeoff elsewhere. Every applied optimization runs repository correctness verification, repeats the representative measurement against the same baseline scenario, and receives independent code review. Report the before/after delta and variance or uncertainty; do not claim success from code shape alone.

If the first diagnosis is not actionable, the human may stop or revise the constraints/workload. A revision gets exactly one additional diagnosis-and-optimization opportunity. There is no retry loop: if the second diagnosis is still not actionable, this flow stops and reports that result.
