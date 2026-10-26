# `findIndex` Operator Behavior

## Operator Behavior in Plain Language

`findIndex` searches source values in order for the first value whose predicate returns true.

It tracks the zero-based source index while suppressing source values. On the first match, it emits that index and completes immediately.

If the source completes without a match, `findIndex` emits `-1` and completes. Source or predicate errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `findIndex` run frame by frame](https://netxpert.ch/rxjs-visualizer/findIndex.html)**. The page runs the RxJS 7.8.2 marble test below, *should return matching element from source emits single element*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should return matching element from source emits single element', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const values = { a: 3, b: 9, c: 15, d: 20 };
    const e1 = hot('  ---a--b--c--d---|', values);
    const e1subs = '  ^--------!       ';
    const expected = '---------(x|)    ';

    const predicate = function (x: number) {
      return x % 5 === 0;
    };

    expectObservable(e1.pipe(findIndex(predicate))).toBe(expected, { x: 2 });
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

The predicate runs on 3, 9 and 15, reported through the probe; 15 at 9 is the first multiple of 5, so its index 2 is emitted there with the completion and the source is unsubscribed before d.

<!-- rxjs-visualizer:end -->
