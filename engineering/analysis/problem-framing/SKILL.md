---
name: problem-framing
description: Recover the real engineering phenomenon from a vague request, and define observable outcomes, scope, facts, assumptions, constraints, and unknowns before designing a solution.
---

# Problem Framing

Use this skill when a request is expressed as a solution, feature, technology, or symptom: “add a cache”, “split this service”, “make it reliable”, or “refactor this module”.

Do not propose implementation yet. Translate the request into the phenomenon that must change.

## Method

1. State the triggering event, affected actor or resource, current undesirable outcome, and desired outcome.
2. Define observable success with metrics, boundaries, and a time or consistency tolerance.
3. Separate facts, assumptions, decisions, constraints, and unknowns. Attach evidence to every material fact.
4. Separate intrinsic constraints from historical, organizational, compatibility, budget, and deadline constraints.
5. Identify scope and non-goals. Record what would count as solving a different problem.
6. List the minimum missing evidence needed for the next design decision.

## Output

```markdown
## Natural-language problem
## Observable success
## Scope and non-goals
## Evidence ledger
| Claim | Type | Evidence | Confidence | Consequence if false |
|---|---|---|---|---|
## Intrinsic constraints
## Extrinsic constraints
## Unknowns and minimum evidence
## Design handoff
```

Never turn a plausible assumption into a fact. If evidence is unavailable, say what small observation or experiment would resolve it.
