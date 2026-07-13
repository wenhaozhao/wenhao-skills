---
name: reliability-reasoning
description: Analyze failures, retries, duplication, reordering, timeouts, partial completion, consistency, and recovery as explicit system behavior.
---

# Reliability Reasoning

Use this skill when work crosses process, service, queue, database, network, or external-system boundaries, or when “reliable” is an acceptance criterion.

Reliability is not the absence of errors. It is a defined behavior when reality is late, duplicated, reordered, unavailable, or unknown.

## Method

1. Identify the durable fact or intent that must not be lost.
2. Map the critical operation and every external or asynchronous boundary.
3. For each boundary, analyze timeout, retry, duplicate delivery, reordering, partial success, and permanent failure.
4. Define ownership, idempotency key, deduplication scope, ordering requirement, and consistency promise.
5. Separate retryable, compensatable, manually repairable, and terminal failures.
6. Define detection, reconciliation, audit, recovery, and operator authority.
7. Test the design with failure injection and measurable recovery objectives.

## Output

```markdown
## Reliability promise
## Durable facts and intents
## Boundary map
| Boundary | Failure modes | Required semantics | Recovery owner |
|---|---|---|---|
## Idempotency and ordering
## Consistency and visibility
## Failure classification
## Recovery and reconciliation
## Observability
## Failure-injection plan
```

Never hide uncertainty behind a generic “retry”. A retry is safe only when its effect, deduplication, and terminal behavior are defined.
