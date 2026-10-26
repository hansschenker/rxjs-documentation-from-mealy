# `publishReplay` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`publishReplay` is a deprecated multicasting operator based on `ReplaySubject`.

It shares one source subscription and stores a configured number or time window of past values. Subscribers can therefore receive buffered past values before receiving new shared values.

Without a selector it returns a connectable Observable that must be connected. With a selector, the replaying shared Observable is scoped to the selector-defined subscription. Source completion and errors are replayed according to the ReplaySubject lifecycle.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `publishReplay` run frame by frame](https://netxpert.ch/rxjs-visualizer/publishReplay.html)**. The page runs the RxJS 7.8.2 marble test below, *should multicast the same values to multiple observers, bufferSize=1*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should multicast the same values to multiple observers, bufferSize=1', () => {
  testScheduler.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const source = cold('    -1-2-3----4-|');
    const sourceSubs = '     ^-----------!';
    const published = source.pipe(publishReplay(1)) as ConnectableObservable<string>;
    const subscriber1 = hot('a|           ').pipe(mergeMapTo(published));
    const expected1 = '      -1-2-3----4-|';
    const subscriber2 = hot('----b|       ').pipe(mergeMapTo(published));
    const expected2 = '      ----23----4-|';
    const subscriber3 = hot('--------c|   ').pipe(mergeMapTo(published));
    const expected3 = '      --------3-4-|';

    expectObservable(subscriber1).toBe(expected1);
    expectObservable(subscriber2).toBe(expected2);
    expectObservable(subscriber3).toBe(expected3);
    expectSubscriptions(source.subscriptions).toBe(sourceSubs);

    published.connect();
  });
});
```

publish through a ReplaySubject(1): a late subscriber first gets the last value replayed, 2 for subscriber2 at 4 and 3 for subscriber3 at 8; subscriber1 joins at 0 before anything was emitted and gets no replay.

<!-- rxjs-visualizer:end -->
