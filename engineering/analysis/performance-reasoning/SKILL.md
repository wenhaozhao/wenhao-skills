---
name: performance-reasoning
description: Diagnose performance problems from measurements, causal critical paths, workload shape, and resource saturation before proposing optimizations.
---

# Performance Reasoning

Use this skill when a system is slow, expensive, overloaded, or unable to meet a latency, throughput, or capacity target.

Do not optimize a named component before showing that it contributes to the observed outcome.

## Method

1. Restate the user-visible performance phenomenon and define the target, including percentile, workload, and time window.
2. Collect evidence for latency distribution, throughput, concurrency, call graph, per-stage timing, errors, retries, data volume, cardinality, and resource saturation.
3. Separate workload changes, contention, queueing, serialization, remote calls, storage access, computation, and measurement artifacts.
4. Map the critical path and distinguish necessary work from delay-tolerant consequences.
5. State bottleneck hypotheses and the evidence that would confirm or falsify each one.
6. Generate at least two interventions, comparing expected effect, correctness risk, operational cost, and reversibility.
7. Define a benchmark or production experiment with guardrails, rollback, and revisit triggers.

## Output

```markdown
## Performance phenomenon
## Target and workload
## Evidence
| Measure | Value/distribution | Scope and time window | Source | Confidence |
|---|---|---|---|---|
## Critical path
## Bottleneck hypotheses
| Hypothesis | Supporting evidence | Falsifying observation | Confidence |
|---|---|---|---|
## Necessary versus delay-tolerant work
## Intervention options
## Selected experiment
## Metrics and guardrails
## Rollback and revisit triggers
```

Require distributions rather than averages when tail latency matters. Treat cache-hit rate, concurrency, and data cardinality as assumptions until measured. A faster response that violates freshness, authorization, or correctness is not a performance success.
