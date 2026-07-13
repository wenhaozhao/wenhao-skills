---
name: domain-modeling-analysis
description: Recover the smallest domain model that explains a system's facts, ownership, invariants, lifecycle, events, commands, and sources of truth.
---

# Domain Modeling

Use this skill after the problem is framed, or whenever a design depends on state, ownership, business rules, or data consistency.

Model the phenomenon, not the existing database tables or service boundaries.

## Method

1. Identify actors, resources, entities with identity, and values without identity.
2. Distinguish commands or intentions from events that have already occurred.
3. Identify the authoritative owner of each mutable fact and the authority allowed to change it.
4. State invariants: uniqueness, conservation, authorization, temporal validity, monotonicity, and idempotency.
5. Enumerate genuine lifecycle states and valid transitions. Include duplicate, timeout, retry, reorder, and partial-failure paths where relevant.
6. Separate canonical facts from projections, caches, indexes, and notifications. Describe rebuild and repair paths.
7. Identify uncertainty boundaries and the minimum information required for each important decision.

## Output

```markdown
## Actors and resources
## Entities and values
## Commands and events
## Ownership and authority
## Invariants
## Lifecycle
```text
state --event / condition--> state
```
## Truth and derivation
| Representation | Role | Owner | Rebuildable? | Freshness/consistency |
|---|---|---|---|---|
## Uncertainty boundaries
## Minimum decision set
## Model risks and open questions
```

Prefer one source of truth plus rebuildable derived views. Do not create a new entity, state, or event unless it explains a real distinction or preserves an invariant.
