# `switchMapTo` Operator Behavior

## Operator Behavior in Plain Language

`switchMapTo` applies `switchMap`'s only-latest policy while mapping every source value to the same supplied inner Observable.

Each source value starts a new subscription to that inner and cancels the previous active inner subscription, if one exists. Only values from the newest subscription are forwarded.

If the source completes while the latest inner is active, the result waits for that inner to complete. Source or active-inner errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `switchMapTo` run frame by frame](https://netxpert.ch/rxjs-visualizer/switchMapTo.html)**. The page runs the RxJS 7.8.2 marble test below, *should map-and-flatten each item to an Observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should map-and-flatten each item to an Observable', () => {
  testScheduler.run(({ hot, cold, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --1-----3--5-------|');
    const e1subs = '  ^------------------!';
    const e2 = cold('   x-x-x|            ', { x: 10 });
    //                        x-x-x|
    //                           x-x-x|
    const expected = '--x-x-x-x-xx-x-x---|';
    const values = { x: 10 };

    const result = e1.pipe(switchMapTo(e2));

    expectObservable(result).toBe(expected, values);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

switchMap with the same inner for every value: 5 at 11 cuts the inner that 3 started at 8 after two of its values, so there is no `xx` pair at 13 as with mergeMapTo.

<!-- rxjs-visualizer:end -->
