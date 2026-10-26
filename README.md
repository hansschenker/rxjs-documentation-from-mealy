# RxJS Documentation from Mealy

Human-readable Operator Behavior documentation for the **113 runtime operators exported by RxJS 7.8.2** from `src/operators/index.ts`.

## Documentation workflow

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

The final document is intentionally the last layer. A user should be able to understand the detailed behavior of an operator without having to read the underlying Mealy state table.

> **Analyze formally. Document behaviorally.**

## Scope

- Baseline: **RxJS 7.8.2**
- Canonical inventory: `ReactiveX/rxjs@7.8.2/src/operators/index.ts`
- 113 runtime pipeable operator exports
- Deprecated aliases that are still exported in 7.8.2 are included
- One Markdown file per operator under `operators/`

The descriptions focus on:

- what values flow over time;
- what happens when new values arrive;
- remembered state;
- concurrency and cancellation policy;
- time/scheduler policy where relevant;
- completion and error behavior.

See [INDEX.md](INDEX.md) for the complete operator inventory.

## Intended role

The Mealy model and State Table are the semantic analysis layer. BDD is the readable projection layer. The files in this repository are the **final user-facing outcome**: detailed Operator Behavior in plain language.
