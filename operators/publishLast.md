# `publishLast` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`publishLast` is a deprecated multicasting operator based on `AsyncSubject`.

Subscribers share one source subscription after the connectable is connected. Source values are remembered but are not emitted immediately by the AsyncSubject.

When the source completes normally, only the final source value is emitted to subscribers and then the shared stream completes. If the source errors, the error is forwarded instead of a final value.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `publishLast` run frame by frame](https://netxpert.ch/rxjs-visualizer/publishLast.html)**. The page runs the RxJS 7.8.2 marble test below, *should multicast the same values to multiple observers*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should multicast the same values to multiple observers', () => {
  testScheduler.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const source = cold('    -1-2-3----4-|   ');
    const sourceSubs = '     ^-----------!   ';
    const published = source.pipe(publishLast()) as ConnectableObservable<string>;
    const subscriber1 = hot('a|              ').pipe(mergeMapTo(published));
    const expected1 = '      ------------(4|)';
    const subscriber2 = hot('----b|          ').pipe(mergeMapTo(published));
    const expected2 = '      ------------(4|)';
    const subscriber3 = hot('--------c|      ').pipe(mergeMapTo(published));
    const expected3 = '      ------------(4|)';

    expectObservable(subscriber1).toBe(expected1);
    expectObservable(subscriber2).toBe(expected2);
    expectObservable(subscriber3).toBe(expected3);
    expectSubscriptions(source.subscriptions).toBe(sourceSubs);

    published.connect();
  });
});
```

publish through an AsyncSubject: nothing is passed on until the source completes, then every subscriber, whenever it joined, gets the last value with the completion at 12.

<!-- rxjs-visualizer:end -->
