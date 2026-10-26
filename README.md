# RxJS Documentation from Mealy

Human-readable operator behavior documentation for the **113 runtime operators exported by RxJS 7.8.2** from `src/operators/index.ts`.

## Method

Each final description follows the documentation pipeline developed for this project:

```text
RxJS Operator
    ↓
Mealy Analysis
    ↓
State Table
    ↓
BDD Formulation
    ↓
Plain-Language Operator Behavior
```

The public document is intentionally the last layer: a user should be able to understand an operator's detailed behavior without having to read a Mealy state table.

**Principle:** Analyze formally. Document behaviorally.

## Scope

- Baseline: RxJS 7.8.2
- Inventory source: `ReactiveX/rxjs@7.8.2/src/operators/index.ts`
- Includes deprecated runtime operator aliases that are still exported in 7.8.2.
- One Markdown file per operator under `operators/`.

The descriptions focus on what flows over time, what happens when new values or lifecycle signals arrive, what state the operator must remember, and its completion/error/cancellation behavior where relevant.
