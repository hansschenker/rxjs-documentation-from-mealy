# `every` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`every` tests whether all source values satisfy a predicate and emits only one final Boolean result.

Each source value is passed to the predicate. As soon as one value fails, `every` emits `false` and completes immediately because later values cannot change the answer.

If the source completes without a failure, `every` emits `true` and completes; an empty source therefore produces `true`. Source or predicate errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `every` run frame by frame](https://netxpert.ch/rxjs-visualizer/every.html)**. The page runs the RxJS 7.8.2 marble test below, *should return false if only one of elements does not match with predicate*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
function predicate(x: number | string) {
  return +x % 5 === 0;
}

it('should return false if only one of elements does not match with predicate', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const values = { a: 5, b: 10, c: 15, d: 18, e: 20 };
    const e1 = hot('  --a--b--c--d--e--|', values);
    const e1subs = '  ^----------!      ';
    const expected = '-----------(x|)   ';

    expectObservable(e1.pipe(every(predicate))).toBe(expected, { x: false });
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

The predicate runs on 5, 10, 15 and 18, reported through the probe; 18 fails it at 11, so false is emitted there with the completion and the source is unsubscribed before e at 14.

<!-- rxjs-visualizer:end -->
