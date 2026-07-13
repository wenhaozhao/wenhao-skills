---
name: migration-design
description: Design reversible migrations of data, ownership, schemas, behavior, or traffic with explicit compatibility, backfill, cutover, repair, and rollback semantics.
---

# Migration Design

Use this skill when changing a schema, source of truth, service boundary, data representation, ownership model, or production traffic path without being able to switch everything at once.

A migration is a lifecycle with intermediate states, not a one-time deployment command.

## Method

1. Define the invariant and user-visible outcome that must hold throughout the migration.
2. Identify the old and new representations, their owners, compatibility window, and canonical source of truth at each phase.
3. Map readers, writers, derived views, external consumers, and data dependencies.
4. Define expand, backfill, dual-read/write if needed, verify, cutover, contract, and cleanup phases.
5. Specify ordering, idempotency, replay, rate limits, consistency lag, and repair behavior.
6. Define cutover gates, observability, rollback direction, and what happens to data written during rollback.
7. Prefer a plan with reversible traffic or ownership changes; if rollback is destructive, document the compensating repair explicitly.

## Output

```markdown
## Migration outcome and invariant
## Old and new forms
| Concern | Current form | Target form | Temporary compatibility |
|---|---|---|---|
## Ownership and source of truth by phase
## Consumers and dependency map
## Migration phases
| Phase | Reads | Writes | Verification gate | Rollback |
|---|---|---|---|---|
## Backfill, replay, and repair
## Consistency and failure semantics
## Cutover plan
## Observability and abort thresholds
## Rollback and data reconciliation
## Cleanup and completion criteria
```

Do not declare a migration complete when code has switched but old data, consumers, repair paths, or operational fallbacks remain unaccounted for.
