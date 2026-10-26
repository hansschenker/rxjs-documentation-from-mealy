# `concatMapTo` Operator Behavior

## Operator Behavior in Plain Language

`concatMapTo` has the same queueing policy as `concatMap`, but every source value maps to the same supplied inner Observable.

A source value starts a subscription to that inner when no inner is active. Further source values are queued while the current inner runs, and each queued value causes another subscription to the same inner after the previous subscription completes.

The result completes only after the source has completed and all queued inner subscriptions finish. Errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `concatMapTo` run frame by frame](https://netxpert.ch/rxjs-visualizer/concatMapTo.html)**. The page runs the RxJS 7.8.2 marble test below, *should map-and-flatten each item to an Observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should map-and-flatten each item to an Observable', () => {
  testScheduler.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --1-----3--5-------|');
    const e1subs = '  ^------------------!';
    const e2 = cold(' x-x-x|              ', { x: 10 });
    const expected = '--x-x-x-x-x-xx-x-x-|';
    const values = { x: 10 };

    const result = e1.pipe(concatMapTo(e2));

    expectObservable(result).toBe(expected, values);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

concatMap with the same inner for every value: the inner for 3 starts at 8 as 3 arrives, but the one for 5 waits until it completes at 13 (a queued span), so its values come at 13, 15 and 17.

<!-- rxjs-visualizer:end -->
