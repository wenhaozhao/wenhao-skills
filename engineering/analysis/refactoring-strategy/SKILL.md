---
name: refactoring-strategy
description: Recover the stable behavior and ownership of a difficult code area, distinguish essential from accidental complexity, and plan a safe incremental refactoring.
---

# Refactoring Strategy

Use this skill when code is hard to change, responsibilities are tangled, behavior is duplicated, or technical debt is blocking a concrete outcome.

Refactoring is a behavior-preserving change unless an intentional behavior change is explicitly recorded.

## Method

1. Name the concrete outcome the refactoring enables; do not refactor for aesthetic cleanliness alone.
2. Establish current behavior from tests, callers, traces, data contracts, and production evidence. Mark undocumented behavior as an assumption.
3. Identify responsibilities, mutable state, ownership, lifecycle, and boundaries currently mixed together.
4. Separate essential complexity required by domain rules from accidental complexity caused by representation, coupling, duplication, or historical compatibility.
5. Define seams around ownership or decision boundaries, not arbitrary file sizes.
6. Generate at least two sequencing options and compare risk, intermediate correctness, migration effort, and reversibility.
7. Plan small behavior-preserving steps, each with a verification point and a safe stopping state.

## Output

```markdown
## Refactoring outcome
## Behavior contract
## Evidence and assumptions
## Responsibility map
| Responsibility | Current location | True owner | Coupling/risk |
|---|---|---|---|
## Essential complexity
## Accidental complexity
## Candidate seams
## Sequencing options
## Selected plan
| Step | Preserved behavior | Verification | Safe stopping state |
|---|---|---|---|
## Rollback and cleanup
## Success and revisit triggers
```

Do not rewrite a subsystem to discover its design. Preserve a working path, expose one seam at a time, and remove duplication only after the authoritative behavior is clear.
