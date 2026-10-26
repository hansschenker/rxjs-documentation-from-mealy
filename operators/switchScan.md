# `switchScan` Operator Behavior

## Operator Behavior in Plain Language

`switchScan` combines accumulated state with `switchMap` cancellation behavior.

For each source value, the accumulator is called with the current accumulated state and returns an inner Observable. A new source value cancels the currently active inner before the newly projected inner becomes active.

Each value from the active inner becomes the new accumulated state and is emitted downstream. Cancelled inners can no longer update the state. Source completion waits for the active inner to finish; source, accumulator, or active-inner errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `switchScan` run frame by frame](https://netxpert.ch/rxjs-visualizer/switchScan.html)**. The page runs the RxJS 7.8.2 marble test below, *should map-and-flatten each item to an Observable while passing the accumulated value*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should map-and-flatten each item to an Observable while passing the accumulated value', () => {
  testScheduler.run(({ hot, cold, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --1-----3--5-------|');
    const e1subs = '  ^------------------!';
    const e2 = cold('    x-x-x|           ', { x: 10 });
    //                        x-x-x|
    //                           x-x-x|
    const expected = '--x-x-x-y-yz-z-z---|';
    const values = { x: 10, y: 40, z: 90 };

    const result = e1.pipe(switchScan((acc, x) => e2.pipe(map((i) => i * Number(x) + acc)), 0));

    expectObservable(result).toBe(expected, values);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

switchMap with an accumulator: each inner's values are i * x + acc, where acc is the last value the previous inner emitted (10 after the first, 40 after the second), and a new outer value switches the running inner off (5 at 11 cuts the second after two values). The runnable entry splits the accumulator into `project` (i * x, what the inner rows show) and `flatten` (+ acc, what reaches the output), so the rows carry 10, 30 and 50 and the output lane 10, 40 and 90.

<!-- rxjs-visualizer:end -->
