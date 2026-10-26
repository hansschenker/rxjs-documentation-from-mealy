# `share` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`share` turns one source execution into a reference-counted shared execution through a connector Subject.

The first downstream subscriber causes a source subscription to be created. Further subscribers receive values from that same Subject-backed execution rather than creating independent source executions.

When subscribers leave, the reference count is updated. By default the shared state resets when the source errors, completes, or the reference count falls to zero, although `ShareConfig` can change those reset policies and the connector type.

Source values and lifecycle notifications are multicast to the current subscribers.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `share` run frame by frame](https://netxpert.ch/rxjs-visualizer/share.html)**. The page runs the RxJS 7.8.2 marble test below, *should share the same values to multiple observers*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
const options = {};

it('should share the same values to multiple observers', () => {
  rxTest.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const source = cold('    -1-2-3----4-|');
    const sourceSubs = '     ^-----------!';
    const subscriber1 = hot('a|           ');
    const expected1 = '      -1-2-3----4-|';
    const subscriber2 = hot('----b|       ');
    const expected2 = '      -----3----4-|';
    const subscriber3 = hot('--------c|   ');
    const expected3 = '      ----------4-|';

    const shared = source.pipe(share(options));

    expectObservable(subscriber1.pipe(mergeMapTo(shared))).toBe(expected1);
    expectObservable(subscriber2.pipe(mergeMapTo(shared))).toBe(expected2);
    expectObservable(subscriber3.pipe(mergeMapTo(shared))).toBe(expected3);
    expectSubscriptions(source.subscriptions).toBe(sourceSubs);
  });
});
```

// The selected case from the upstream parameterized suite:

Entry 47. Three subscribers join at 0, 4 and 8, each through its own hot and mergeMapTo; the cold source is subscribed exactly once (one bar), and a late subscriber gets only what comes after it joined.

<!-- rxjs-visualizer:end -->
