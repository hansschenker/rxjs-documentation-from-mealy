# `switchMap` Operator Behavior

## Operator Behavior in Plain Language

`switchMap` starts with no active inner stream.

When an outer value arrives, that value is projected to an inner stream and `switchMap` subscribes to it.

While that inner is active, its values are forwarded downstream.

If another outer value arrives before the current inner completes, `switchMap` unsubscribes from the current inner and replaces it with the newly projected inner.

Only the currently active inner may contribute values.

If an inner completes while the outer is still running, `switchMap` returns to a state with no active inner and waits for another outer value.

If the outer completes while an inner is still active, `switchMap` allows that inner to continue. The result completes only after that final active inner completes.

An error from the outer stream or active inner stream terminates the operator with an error.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `switchMap` run frame by frame](https://netxpert.ch/rxjs-visualizer/switchMap.html)**. The page runs the RxJS 7.8.2 marble test below, *should map-and-flatten each item to an Observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should map-and-flatten each item to an Observable', () => {
  testScheduler.run(({ hot, cold, expectObservable, expectSubscriptions }) => {
    const e1 = hot('   --1-----3--5-------|');
    const e1subs = '   ^------------------!';
    const e2 = cold('    x-x-x|            ', { x: 10 });
    //                         x-x-x|
    //                            x-x-x|
    const expected = ' --x-x-x-y-yz-z-z---|';
    const values = { x: 10, y: 30, z: 50 };

    const result = e1.pipe(switchMap((x) => e2.pipe(map((i) => i * +x))));

    expectObservable(result).toBe(expected, values);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

The second inner (for 3) is unsubscribed at frame 11, when 5 arrives, before its own completion: its lane ends with `!` and no complete bar.

<!-- rxjs-visualizer:end -->
