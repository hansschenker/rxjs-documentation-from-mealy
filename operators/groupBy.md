# `groupBy` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`groupBy` partitions source values into keyed `GroupedObservable` streams.

For each source value it computes a key. The first value for a new key creates a new group, emits that group from the outer result, and routes the value into it. Later values with the same key are routed to the existing group.

Optional element, duration, and connector functions can transform group values, close groups, or choose the Subject used by each group. Source completion completes all open groups and the outer result. Source or user-function errors error the affected grouping process.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `groupBy` run frame by frame](https://netxpert.ch/rxjs-visualizer/groupBy.html)**. The page runs the RxJS 7.8.2 marble test below, *should group numbers by odd/even*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should group numbers by odd/even', () => {
  testScheduler.run(({ cold, hot, expectObservable }) => {
    const e1 = hot('  --1---2---3---4---5---|');
    const expected = '--x---y---------------|';
    const x = cold('  1-------3-------5---|');
    const y = cold('  2-------4-------|');
    const expectedValues = { x: x, y: y };

    const source = e1.pipe(groupBy((val: string) => parseInt(val) % 2));
    expectObservable(source).toBe(expected, expectedValues);
  });
});
```

Reserve list, groupBy-spec.ts at tag 7.8.2, the first test. The output values are the groups themselves, which the TestScheduler materializes on both sides; `expectedInners` carries the colds. The output lane names each group by its key (1 for odd, 0 for even) and each group is a row of its own values under it, opened at 2 and 6.

<!-- rxjs-visualizer:end -->
