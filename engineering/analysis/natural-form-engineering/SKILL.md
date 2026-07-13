---
name: natural-form-engineering
description: Analyze software requirements, architecture changes, performance problems, refactoring, reliability incidents, and technical debt by recovering the problem's natural form: facts, invariants, causal relationships, ownership, lifecycle, uncertainty, and evidence. Use before proposing implementation patterns or technologies.
---

# Natural Form Engineering

Use this skill when facing a business requirement, architecture design, performance optimization, reliability problem, refactoring task, legacy-system governance, or major technical choice.

The purpose is not to find a philosophically “perfect” design. The purpose is to:

1. recover the real problem from its wording and existing implementation;
2. identify intrinsic constraints and causal structure;
3. separate essential complexity from accidental complexity;
4. derive the smallest coherent design;
5. test the design against evidence and failure conditions;
6. produce an executable engineering decision.

## Core principle

Do not begin with frameworks, patterns, modules, or technologies.

Begin with:

- facts;
- actors and resources;
- state and lifecycle;
- invariants;
- causal and temporal relations;
- ownership and decision authority;
- uncertainty and boundaries;
- measurable outcomes.

Treat architecture as a model discovered under constraints, not a structure invented from taste.

Natural form does not mean “whatever already exists”, “minimal code”, “no design”, or “copying the real world literally”. A good model is a deliberate compression of reality for a specific purpose.

## Mandatory behavior

Before proposing a solution:

1. Restate the task without implementation language.
2. Identify the desired outcome and observable success criteria.
3. List intrinsic constraints separately from historical or organizational constraints.
4. Identify sources of truth, derived data, and duplicated representations.
5. Model important state transitions, causality, ownership, and uncertainty.
6. Find the minimum decision set: the smallest information required to make each important decision correctly.
7. Locate evidence: code, traces, metrics, database facts, tests, domain rules, or experiments.
8. Generate at least two plausible designs when the decision is consequential.
9. Explain which constraints eliminate or weaken alternatives.
10. Define validation, rollout, observability, and rollback.

Never recommend a pattern merely because it is conventional. Never assert a “natural” model without evidence or explicit assumptions.

## Workflow: NATURE

Use the six-stage NATURE workflow.

### N — Name the real phenomenon

Translate the request from solution language into phenomenon language.

Capture:

- triggering event;
- affected actor or resource;
- desired outcome;
- current undesirable outcome;
- scope;
- success metric;
- deadline or latency tolerance.

Examples:

- “Add a delay queue for benefit expiration” becomes:
  “A time-bounded entitlement must cease authorizing use after its validity interval, within an accepted delay.”
- “Add Redis to speed up the endpoint” becomes:
  “The user waits too long for a result composed of information with different costs, volatility, and necessity.”

Output:

```markdown
## Natural-language problem statement
...
## Observable success
...
## Non-goals
...
```

### A — Ascertain facts and assumptions

Collect evidence before modeling.

Classify each statement:

- **Observed fact** — directly supported by data, code, contract, or domain authority.
- **Assumption** — plausible but unverified.
- **Decision** — a chosen policy, not a fact.
- **Constraint** — a restriction that must be respected.
- **Unknown** — material information not yet available.

Do not silently upgrade assumptions into facts.

For performance work, require:

- latency distribution rather than only average;
- throughput and concurrency;
- call graph or critical path;
- per-stage timing;
- resource saturation;
- data volume and cardinality;
- cache-hit assumptions;
- error and retry behavior.

For business design, require:

- real business event;
- source of truth;
- actor with authority;
- lifecycle;
- money, inventory, quota, permission, or entitlement invariants;
- duplicate, timeout, reorder, partial-failure, and retry semantics.

### T — Trace constraints and relationships

Build a constraint map.

#### Intrinsic constraints

These arise from the phenomenon:

- conservation: money, inventory, quota, capacity;
- uniqueness;
- monotonicity;
- causality;
- temporal validity;
- idempotent effect;
- authorization;
- ownership;
- auditability;
- legal or contractual obligation;
- physical or external-system limits.

#### Extrinsic constraints

These arise from the current environment:

- legacy schema;
- team boundary;
- current RPC;
- selected cloud vendor;
- deployment window;
- compatibility requirement;
- budget;
- migration capacity.

Do not ignore extrinsic constraints, but do not allow them to define the core domain model unless they truly belong there.

Map important relationships:

- cause → effect;
- must precede;
- may happen independently;
- mutually exclusive;
- contains / belongs to;
- owns / observes;
- source → projection;
- synchronous necessity / asynchronous consequence;
- stable / volatile;
- deterministic / uncertain.

Use tables or diagrams only when they clarify a decision.

### U — Uncover the natural model

Recover the smallest model that explains the facts and preserves the invariants.

Identify:

- entities or resources with identity;
- values without identity;
- events that have occurred;
- commands or intentions;
- policies that decide;
- states and transitions;
- boundaries where uncertainty enters;
- single owners of mutable state;
- canonical source of truth;
- projections and caches;
- stable core versus volatile edges.

Ask:

1. What actually changes?
2. What must never become false?
3. Who has authority to decide?
4. What is a fact, and what is a consequence of that fact?
5. Which states are genuine business states rather than exception-handling artifacts?
6. What information is minimally sufficient for the decision?
7. Which representations can be derived instead of jointly maintained?
8. Which work is essential on the critical path?
9. Where does reality become uncertain or eventually consistent?
10. What complexity belongs to the problem, and what complexity was created by the implementation?

Prefer explicit state models over scattered conditionals when lifecycle matters.

Prefer one source of truth plus derived projections over multiple coordinated truths.

Prefer ownership over broad shared mutation.

Prefer recording durable intent before performing retryable external effects.

### R — Resolve into design options

Generate options from the model rather than jumping to a favorite technology.

For each option, document:

- model and responsibility boundaries;
- data ownership;
- transaction boundary;
- synchronization and ordering;
- failure semantics;
- retry and idempotency;
- consistency model;
- critical path;
- observability;
- migration cost;
- reversibility;
- operational burden.

Use patterns only as implementation vocabulary after the need is established:

- cache when reuse and temporal locality exist;
- events when one fact has independent, delay-tolerant consequences;
- queue when work must be buffered, retried, rate-limited, or decoupled in time;
- state machine when valid transitions and lifecycle are central;
- actor or single writer when mutable state needs clear ownership;
- type-level constraints when invalid states can be made unrepresentable;
- precomputation when expensive work can be shifted before demand;
- materialized projection when reads need a purpose-specific derived form.

Select the option that best fits the constraints with the least accidental mechanism, not necessarily the fewest components.

### E — Experiment, evaluate, and evolve

A design is incomplete until it can be disproved.

Define:

- acceptance tests;
- invariant/property tests;
- benchmark or load-test design;
- failure injection;
- shadow traffic or replay;
- staged rollout;
- metrics and alerts;
- rollback;
- post-rollout review date;
- evidence that would cause redesign.

Prefer the smallest reversible experiment that distinguishes competing explanations.

For performance optimization, optimize only after measurement and re-measure after each meaningful change.

For architecture change, include a migration path that keeps old and new models coherent without long-lived dual ownership.

## Task-specific modes

### Mode A: Business requirement

Produce:

1. phenomenon statement;
2. domain facts and assumptions;
3. actors, resources, and authority;
4. invariants;
5. lifecycle/state transitions;
6. source of truth and derived views;
7. synchronous core and asynchronous consequences;
8. failure and compensation semantics;
9. minimal coherent design;
10. acceptance scenarios.

### Mode B: Performance optimization

Use this sequence:

1. Define user-visible objective: P50/P95/P99, throughput, cost, or capacity.
2. Measure the end-to-end critical path.
3. Decompose total latency:
   `T = queueing + compute + I/O + coordination + retries + unnecessary work`.
4. Classify each output component by:
   - necessity;
   - volatility;
   - reuse;
   - computation cost;
   - consistency requirement;
   - dependency.
5. Find structural waste:
   - work not required for the decision;
   - accidental serialization;
   - repeated calculation or fetching;
   - excessive coordination;
   - mismatched data shape;
   - unbounded fan-out;
   - retries amplifying load;
   - queueing caused by saturation.
6. Form hypotheses with predicted metric changes.
7. Test the highest-information, lowest-risk hypothesis first.
8. Verify tail latency, correctness, resource use, and behavior under load.

Do not start with “add cache”, “increase threads”, “make asynchronous”, or “change database”.

### Mode C: Refactoring and technical debt

Recover:

- the behavior that must remain;
- the invariant currently hidden in code;
- duplicated sources of truth;
- ownership ambiguity;
- unstable dependencies;
- change hotspots;
- accidental abstractions;
- migration seams.

Refactor toward clearer truth, ownership, and constraints. Do not reorganize code without reducing a demonstrated source of change cost or risk.

### Mode D: Incident or reliability problem

Separate:

- triggering event;
- latent condition;
- propagation path;
- missing containment;
- recovery mechanism;
- observability gap.

Do not stop at the failed component. Model how local failure became user-visible impact.

## Anti-pattern checks

Before finalizing, check whether the proposal:

- models the requested implementation instead of the real phenomenon;
- confuses current process with domain truth;
- creates multiple writable sources of truth;
- relies on developers remembering hidden rules;
- treats normal uncertainty as an exception;
- introduces asynchronous behavior without ownership and delivery semantics;
- introduces cache without staleness and invalidation semantics;
- introduces generic abstractions before stable variation exists;
- optimizes average latency while worsening tail latency;
- increases degrees of freedom where the domain requires restriction;
- makes irreversible decisions without sufficient evidence;
- calls a subjective preference “natural”.

If any check fails, revise or state the trade-off explicitly.

## Required final output

Use the following structure, adapting depth to task size:

```markdown
# Engineering analysis

## 1. Natural form
- Phenomenon:
- Desired outcome:
- Observable success:
- Non-goals:

## 2. Evidence ledger
| Claim | Type | Evidence | Confidence |
|---|---|---|---|

## 3. Constraint map
### Intrinsic
...
### Extrinsic
...

## 4. Causal and state model
...

## 5. Sources of truth and ownership
...

## 6. Essential vs accidental complexity
...

## 7. Design options
### Option A
...
### Option B
...

## 8. Decision
- Chosen option:
- Why constraints favor it:
- Costs accepted:
- Assumptions remaining:

## 9. Validation and rollout
...

## 10. Revisit conditions
...
```

For small tasks, compress the output while preserving the reasoning sequence.

## Interaction rules

- Ask clarifying questions only when the missing answer blocks safe progress. Otherwise state assumptions and proceed.
- Inspect the repository, metrics, traces, schema, and tests when available.
- Prefer concrete evidence to conceptual elegance.
- Use domain language consistently.
- Make uncertainty visible.
- Challenge the request respectfully when it prescribes a solution unsupported by the problem.
- End with an executable decision, experiment, or next engineering action.
