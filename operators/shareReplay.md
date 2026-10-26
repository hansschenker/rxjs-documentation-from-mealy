# `shareReplay` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`shareReplay` shares one source execution and replays buffered past values to later subscribers through a `ReplaySubject`.

The configured buffer size and time window determine what history a new subscriber receives before current shared values. A successfully completed source remains cached, so later subscribers can receive the replayed values and completion without rerunning the source.

After an error the shared state resets so a later subscriber can create a new source execution. By default `refCount` is false, so the source can remain connected even with no subscribers; with `refCount: true`, dropping to zero disconnects and resets the shared execution.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `shareReplay` run frame by frame](https://netxpert.ch/rxjs-visualizer/shareReplay.html)**. The page runs the RxJS 7.8.2 marble test below, *should multicast the same values to multiple observers, bufferSize=1*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should multicast the same values to multiple observers, bufferSize=1', () => {
  testScheduler.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const source = cold('    -1-2-3----4-|');
    const sourceSubs = '     ^-----------!';
    const subscriber1 = hot('a|           ');
    const expected1 = '      -1-2-3----4-|';
    const subscriber2 = hot('----b|       ');
    const expected2 = '      ----23----4-|';
    const subscriber3 = hot('--------c|   ');
    const expected3 = '      --------3-4-|';

    const shared = source.pipe(shareReplay(1));

    expectObservable(subscriber1.pipe(mergeMapTo(shared))).toBe(expected1);
    expectObservable(subscriber2.pipe(mergeMapTo(shared))).toBe(expected2);
    expectObservable(subscriber3.pipe(mergeMapTo(shared))).toBe(expected3);
    expectSubscriptions(source.subscriptions).toBe(sourceSubs);
  });
});
```

Entry 48. The same timings as the share spec; the difference is on the late subscribers' lanes: subscriber2 gets the remembered 2 the moment it joins at 4, subscriber3 the remembered 3 at 8. Still one source bar.

<!-- rxjs-visualizer:end -->
