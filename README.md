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

## Visualizations

Every operator page ends with a **Visualization** section: the RxJS 7.8.2 marble test that defines the operator's behavior, quoted verbatim, and a link to the same test animated frame by frame on the [RxJS Visualizer](https://netxpert.ch/rxjs-visualizer/), which runs it through the real `TestScheduler` and draws every frame: the inputs, the operator's windows and inner subscriptions, expected values as rings and actual values as fills. Read the plain-language behavior first, then watch the test run. Deprecated aliases (`flatMap`, `combineAll`, `exhaust`, and the operator forms of `concat`, `merge`, `zip`, `race`, `combineLatest`) point at the operator they stand for.

## Intended role

The Mealy model and State Table are the semantic analysis layer. BDD is the readable projection layer. The files in this repository are the **final user-facing outcome**: detailed Operator Behavior in plain language.
