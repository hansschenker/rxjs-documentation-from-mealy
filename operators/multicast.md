# `multicast` Operator Behavior

## Operator Behavior in Plain Language

`multicast` is a deprecated multicasting operator that routes one source subscription through a Subject or Subject factory.

Without a selector, it produces a connectable Observable: downstream subscribers attach to the Subject, but the source does not start until the connectable is explicitly connected.

With a selector, the Subject-backed shared Observable is passed into the selector and the resulting dataflow is connected to the source for that subscription. Source and selector lifecycle signals flow through that shared execution.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `multicast` run frame by frame](https://netxpert.ch/rxjs-visualizer/multicast.html)**. The page runs the RxJS 7.8.2 marble test below, *should multicast the same values to multiple observers*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should multicast the same values to multiple observers', () => {
  testScheduler.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const e1 = cold('        -1-2-3----4-|');
    const e1subs = '         ^-----------!';
    const multicasted = e1.pipe(multicast(() => new Subject<string>())) as ConnectableObservable<string>;
    const subscriber1 = hot('a|           ').pipe(mergeMapTo(multicasted));
    const expected1 = '      -1-2-3----4-|';
    const subscriber2 = hot('----b|       ').pipe(mergeMapTo(multicasted));
    const expected2 = '      -----3----4-|';
    const subscriber3 = hot('--------c|   ').pipe(mergeMapTo(multicasted));
    const expected3 = '      ----------4-|';

    expectObservable(subscriber1).toBe(expected1);
    expectObservable(subscriber2).toBe(expected2);
    expectObservable(subscriber3).toBe(expected3);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);

    multicasted.connect();
  });
});
```

The general form behind publish: the subject factory says what the subscribers share through, a plain Subject here, so the picture is publish's; publishBehavior, publishLast and publishReplay are multicast with a BehaviorSubject, an AsyncSubject and a ReplaySubject.

<!-- rxjs-visualizer:end -->
