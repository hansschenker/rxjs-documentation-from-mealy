# `scan` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`scan` maintains accumulated state and emits the new state after each source value.

With a seed, the first source value is combined with that seed. Without a seed, the first source value becomes the accumulated state and is emitted as that state; later values are passed to the accumulator with the current state and index.

Every successful accumulation produces the next downstream value, so the state evolves as a stream. Source completion completes the result, and source or accumulator errors terminate it.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `scan` run frame by frame](https://netxpert.ch/rxjs-visualizer/scan.html)**. The page runs the RxJS 7.8.2 marble test below, *should scan*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should scan', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    // prettier-ignore
    const values = {
      a: 1, b: 3, c: 5,
      x: 1, y: 4, z: 9,
    };
    const e1 = hot('  --a--b--c--|', values);
    const e1subs = '  ^----------!';
    const expected = '--x--y--z--|';

    const scanFunction = function (o: number, x: number) {
      return o + x;
    };

    expectObservable(e1.pipe(scan(scanFunction, 0))).toBe(expected, values);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 09. One values map serves both the source (a, b, c) and the expected output (x, y, z), so both lanes list all six.

<!-- rxjs-visualizer:end -->
