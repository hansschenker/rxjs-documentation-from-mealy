# `publishBehavior` Operator Behavior

## Operator Behavior in Plain Language

`publishBehavior` is a deprecated multicasting operator based on `BehaviorSubject`.

It creates a connectable Observable whose subscribers share one source subscription after `connect()` is called. Before the source has emitted, subscribers receive the configured initial value; later subscribers receive the current most recent value from the BehaviorSubject.

Source values, completion, and errors are multicast through that subject.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `publishBehavior` run frame by frame](https://netxpert.ch/rxjs-visualizer/publishBehavior.html)**. The page runs the RxJS 7.8.2 marble test below, *should multicast the same values to multiple observers*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should multicast the same values to multiple observers', () => {
  testScheduler.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const source = cold('   -1-2-3----4-| ');
    const sourceSubs = '    ^-----------! ';
    const published = source.pipe(publishBehavior('0')) as ConnectableObservable<string>;
    const subscriber1 = hot('a|           ').pipe(mergeMapTo(published));
    const expected1 = '      01-2-3----4-|';
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

publish through a BehaviorSubject: every subscriber first gets the current value, the initial '0' for subscriber1 at 0, the latest 2 for subscriber2 at 4 and 3 for subscriber3 at 8, then what follows.

<!-- rxjs-visualizer:end -->
