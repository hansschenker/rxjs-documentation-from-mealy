# `expand` Operator Behavior

## Operator Behavior in Plain Language

`expand` recursively feeds emitted values back through a projection function to create more Observable work.

Each source value is emitted downstream and is also projected to an inner Observable. Values produced by those projected Observables are themselves emitted and recursively projected again.

Several projected branches may be active concurrently, subject to the configured concurrency limit. Completion occurs when the original source is complete and no projected work remains. Source, projection, or inner errors terminate the recursive expansion.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `expand` run frame by frame](https://netxpert.ch/rxjs-visualizer/expand.html)**. The page runs the RxJS 7.8.2 marble test below, *should recursively map-and-flatten each item to an Observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should recursively map-and-flatten each item to an Observable', () => {
  testScheduler.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --x----|  ', { x: 1 });
    const e1subs = '  ^------!  ';
    const e2 = cold('   --c|    ', { c: 2 });
    //                    --c|
    //                      --c|
    const expected = '--a-b-c-d|';
    const values = { a: 1, b: 2, c: 4, d: 8 };

    const result = e1.pipe(expand((x) => (x === 8 ? EMPTY : e2.pipe(map((c) => c * x)))));

    expectObservable(result).toBe(expected, values);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 50. The seed 1 is emitted and projected; each inner's value is emitted and projected again, so inner 2 hangs from inner 1's 2 at 4 and inner 3 from inner 2's 4 at 6. 8 projects to EMPTY, no inner, the end of the recursion. `Number(c) * Number(x)` is the typed spelling of `c * x`.

A second page: **[expandMany](https://netxpert.ch/rxjs-visualizer/expandMany.html)**, *a project function emitting two values per input (not an rxjs test)*.

<!-- rxjs-visualizer:end -->
