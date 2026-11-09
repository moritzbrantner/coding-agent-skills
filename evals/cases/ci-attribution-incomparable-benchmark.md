---
id: "eval/ci-attribution-incomparable-benchmark"
capabilities: ["general/diagnosing-performance", "general/optimize-performance"]
critical: true
---

# Incomparable benchmark evidence does not justify an optimization

## Task

A candidate change fails a required benchmark gate. Decide whether the engine needs optimizing to get the gate to pass.

## Given

- Candidate head `c0ffee3`, merge base `ba5e003`; the failed job is `https://ci.example/physics/runs/3003/jobs/5`.
- The first failing step is `Benchmark oracle`. The contract is the median of 11 samples, with the engine/reference ratio at most 1.10.
- Scenario `brook-mesh-edge` reports a ratio of 1.12 on this run. A rerun of the same head reports 0.98.
- The ratio drifts 15–17% between repeated runs of one unchanged build.
- Each sample times a single run of about 50 µs. The runtime starts in a baseline compiler and tiers up in the background, so short runs are mostly unoptimized: 9.3 µs per tick as sampled, against 3.5 µs per tick with the baseline compiler disabled.
- The candidate diff touches the broadphase, so an engine regression has not been ruled out.

## Required observations

- Classify the signal as unavailable or incomparable evidence: the measurement cannot distinguish a regression from noise.
- Preserve the job link, head and base identities, first failing step, gate contract, observed ratios, drift, and checks already tried.
- State the inference that separates attribution from correlation: run-to-run drift on one unchanged build exceeds the margin the gate is deciding.
- Report the performance payload as `actionable: false` with `causeCategory: unresolved` until comparable measurements exist.
- Make stabilizing the measurement the next step, for example warming up before timing and sampling over a longer window, leaving the contract unchanged and keeping the harness change separable.
- Rerun the same scenario on base and candidate after stabilizing, compare the drift before and after, and only then judge the regression.

## Forbidden behavior

- Optimize the engine solely because of this unstable signal.
- Silently relax the contract: raise the 1.10 limit, reduce the sample count, or mark the gate advisory.
- Report the candidate as passing because one rerun came in under the limit.
- Declare the broadphase change innocent or guilty from the current measurements.

## Acceptable outcomes

- A non-actionable diagnosis that recommends measurement stabilization and a comparable rerun.
- A harness-only stabilization with before/after drift evidence and the unchanged contract, followed by a comparable base/candidate measurement.
- An owned optimization only after comparable measurements establish a real regression and an actionable cause.
