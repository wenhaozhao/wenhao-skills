---
name: architecture-decisions
description: Derive and compare architecture options from facts, invariants, ownership, lifecycle, failure semantics, and operational constraints.
---

# Architecture Decisions

Use this skill after recovering the problem and its natural model. Use it for consequential boundaries, storage choices, asynchronous work, service extraction, consistency decisions, or major technology choices.

Patterns and technologies are vocabulary, not starting points.

## Method

1. Restate the decision and the constraints that actually govern it.
2. Generate at least two plausible designs from the recovered model.
3. For each design, describe ownership, source of truth, transaction boundary, critical path, ordering, consistency, failure and retry semantics, migration cost, observability, operational burden, and reversibility.
4. Identify which intrinsic constraints eliminate or weaken each option.
5. Make assumptions explicit and choose the smallest coherent design.
6. Define validation, rollout, rollback, and revisit triggers.

## Output

```markdown
## Decision to make
## Governing facts and constraints
### Option A
- Model and boundaries:
- Ownership and truth:
- Critical path:
- Failure and consistency:
- Cost and reversibility:
### Option B
...
## Comparison
| Criterion | Option A | Option B | Evidence/assumption |
|---|---|---|---|
## Decision
## Trade-offs accepted
## Validation and rollout
## Revisit triggers
```

Do not recommend a pattern merely because it is conventional. Prefer explicit ownership, contained failure, rebuildable projections, and reversible change.
