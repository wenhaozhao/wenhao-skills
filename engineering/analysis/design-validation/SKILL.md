---
name: design-validation
description: Turn an engineering design into a small, evidence-driven validation plan with experiments, tests, metrics, rollout, rollback, and revisit triggers.
---

# Design Validation

Use this skill after a model or design has been proposed, especially when assumptions are uncertain or change risk is high.

Validation should distinguish competing explanations and expose failure conditions; it is not a ceremonial test checklist.

## Method

1. List the design's material assumptions and the consequences if each is false.
2. Convert each important assumption into a measurable hypothesis.
3. Choose the smallest reversible experiment, test, trace, migration rehearsal, or failure injection that can discriminate between hypotheses.
4. Define functional, consistency, latency, throughput, resource, recovery, and business metrics as applicable.
5. Define rollout stages, guardrails, rollback actions, and data repair requirements.
6. State the evidence threshold for accepting the design and the triggers for revisiting it.

## Output

```markdown
## Design under validation
## Assumptions and risks
| Assumption | If false | Hypothesis | Evidence needed |
|---|---|---|---|
## Smallest useful experiment
## Tests and failure injection
## Metrics and thresholds
## Rollout
## Rollback and repair
## Acceptance decision
## Revisit triggers
```

Prefer a cheap, reversible experiment over a broad implementation when it can answer the same decision. Do not claim validation without evidence.
