# `publish` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`publish` is a deprecated way to multicast a source through a plain `Subject`.

Without a selector, it returns a connectable Observable: subscribers can attach first, and an explicit `connect()` starts the single shared source subscription.

With a selector, the shared Subject-backed Observable is passed to the selector and the selector's dataflow is connected for the lifetime of that subscription. A plain Subject does not replay earlier values to late subscribers.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `publish` run frame by frame](https://netxpert.ch/rxjs-visualizer/publish.html)**. The page runs the RxJS 7.8.2 marble test below, *should multicast the same values to multiple observers*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should multicast the same values to multiple observers', () => {
  testScheduler.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const source = cold('   -1-2-3----4-|');
    const sourceSubs = '    ^-----------!';
    const published = source.pipe(publish()) as ConnectableObservable<string>;
    const subscriber1 = hot('a|           ').pipe(mergeMapTo(published));
    const expected1 = '      -1-2-3----4-|';
    const subscriber2 = hot('----b|       ').pipe(mergeMapTo(published));
    const expected2 = '      -----3----4-|';
    const subscriber3 = hot('--------c|   ').pipe(mergeMapTo(published));
    const expected3 = '      ----------4-|';

    expectObservable(subscriber1).toBe(expected1);
    expectObservable(subscriber2).toBe(expected2);
    expectObservable(subscriber3).toBe(expected3);
    expectSubscriptions(source.subscriptions).toBe(sourceSubs);

    published.connect();
  });
});
```

share's picture with the connection made by hand: `connect()` subscribes the cold at 0, once, and the three subscribers see whatever comes after they join, through a plain Subject: no replay, no initial value.

<!-- rxjs-visualizer:end -->
