# Natural Form Engineering — Practitioner Guide

## One-page field card

When a task arrives, do not ask “which pattern should I use?” Start with:

1. **Phenomenon** — What is actually happening?
2. **Outcome** — What observable result must change?
3. **Facts** — What is known from evidence?
4. **Invariants** — What must never become false?
5. **Causality** — What causes what?
6. **Ownership** — Who may change the state?
7. **Lifecycle** — What are the genuine states and transitions?
8. **Uncertainty** — Where can the answer be late, duplicated, reordered, or unknown?
9. **Minimum decision set** — What is the least information needed to decide correctly?
10. **Experiment** — What small reversible test distinguishes the hypotheses?

## Three-layer recovery

Every task has three layers:

- **Request form**: what someone asked for.
- **Current form**: how the existing system happens to implement it.
- **Natural form**: facts, constraints, lifecycle, causality, and authority inherent to the problem.

Do not move directly from request form to implementation.

## Evidence ledger

| Claim | Type | Evidence | Confidence | Consequence if false |
|---|---|---|---|---|
| “Inventory changes frequently” | Assumption | No production measurement yet | Low | Cache design may be invalid |
| “One payment can settle once” | Invariant | Finance contract | High | Requires idempotent effect |
| “P99 is 1.8 s” | Fact | 7-day trace dashboard | High | Sets optimization baseline |

## Design quality test

A design is stronger when:

- business truth has one authoritative owner;
- invalid transitions are difficult or impossible;
- downstream consequences do not redefine upstream facts;
- uncertainty is explicit;
- volatile details live at the edge;
- the critical path contains only necessary work;
- derived information can be rebuilt;
- failure is contained;
- assumptions are testable;
- migration is reversible.

## “Natural” does not mean

- copying human workflows;
- mirroring database tables;
- accepting legacy structure;
- avoiding abstraction;
- choosing the simplest-looking code;
- refusing deliberate intervention;
- assuming one objectively perfect model exists.

It means choosing a model whose structure is strongly justified by the phenomenon and purpose.
