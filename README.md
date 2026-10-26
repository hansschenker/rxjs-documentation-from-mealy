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

## How to read a page

Each operator page has two parts.

**Operator Behavior in Plain Language** says what the operator does with the values that flow through it: what it emits and when, what it remembers between values, how it handles several things happening at once (concurrency and cancellation), what the scheduler or clock has to do with it, and what happens on completion and on error. Read this first.

**Visualization** shows the same behavior as RxJS itself defines it: the marble test from the RxJS 7.8.2 test suite that pins the operator down, and a link to that test run frame by frame. The test is written in marble syntax, which reads left to right, one character per frame of virtual time:

- `-` is a frame in which nothing happens; letters and digits are values (a `values` map says what they stand for)
- `|` is completion, `#` an error
- `^` is the frame an observable is subscribed, `!` the frame it is unsubscribed; the test asserts these in its `subs` strings
- `(ab|)` puts several events in the same frame
- `hot(...)` is a stream that runs whether or not anyone listens, `cold(...)` starts over for every subscriber, `time('---|')` is a duration in frames

On the linked page every column is one frame. The inputs are the top lanes, the operator's own row sits in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes), and the output lane at the bottom shows the expected values as rings and what actually arrived as fills: when every ring has a fill, the operator did what the test says. The yellow bars are subscriptions, read from the scheduler's own log. Press Start to play, drag the scrubber to any frame, hover a marble for its frame and value; the text under the drawing walks through the same frames with a runnable sample.

## Visualizations

Every operator page ends with a **Visualization** section: the RxJS 7.8.2 marble test that defines the operator's behavior, quoted verbatim, and a link to the same test animated frame by frame on the [RxJS Visualizer](https://netxpert.ch/rxjs-visualizer/), which runs it through the real `TestScheduler` and draws every frame: the inputs, the operator's windows and inner subscriptions, expected values as rings and actual values as fills. Read the plain-language behavior first, then watch the test run. Deprecated aliases (`flatMap`, `combineAll`, `exhaust`, and the operator forms of `concat`, `merge`, `zip`, `race`, `combineLatest`) point at the operator they stand for.

## Intended role

The Mealy model and State Table are the semantic analysis layer. BDD is the readable projection layer. The files in this repository are the **final user-facing outcome**: detailed Operator Behavior in plain language.
