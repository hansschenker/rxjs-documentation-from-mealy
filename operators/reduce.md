# `reduce` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`reduce` accumulates source values into one state but emits no intermediate states.

With a seed, accumulation starts from that seed. Without a seed, the first source value becomes the initial accumulated state and later values are passed to the accumulator with the current state and source index.

When the source completes, the final accumulated state is emitted once and the result completes. If there was no seed and no source value, there is no accumulated value to emit. Source or accumulator errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `reduce` run frame by frame](https://netxpert.ch/rxjs-visualizer/reduce.html)**. The page runs the RxJS 7.8.2 marble test below, *should reduce*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should reduce', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const values = { a: 1, b: 3, c: 5, x: 9 };
    const e1 = hot('  --a--b--c--|   ', values);
    const e1subs = '  ^----------!   ';
    const expected = '-----------(x|)';

    const result = e1.pipe(reduce((o, x) => o + x, 0));

    expectObservable(result).toBe(expected, values);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 32. The same inputs and fold as the scan spec, but only the final state leaves, together with the completion at 11.

<!-- rxjs-visualizer:end -->
