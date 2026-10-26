# `exhaustMap` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`exhaustMap` starts with no active inner stream.

When a source value arrives while idle, the projection function creates an inner Observable and `exhaustMap` subscribes to it. While that inner remains active, later source values are ignored completely: they are neither queued nor used to cancel the current inner.

When the active inner completes, the next future source value may start new work. If the source completes while an inner is active, the result waits for it to finish. Source, projection, or active-inner errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `exhaustMap` run frame by frame](https://netxpert.ch/rxjs-visualizer/exhaustMap.html)**. The page runs the RxJS 7.8.2 marble test below, *should map-and-flatten each item to an Observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should map-and-flatten each item to an Observable', () => {
  testScheduler.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const values = { x: 10, y: 30, z: 50 };
    const e1 = hot('   --1-----3--5-------|');
    const e1subs = '   ^------------------!';
    const e2 = cold('    x-x-x|            ', values);
    //                         x-x-x|
    //                            x-x-x|
    const expected = ' --x-x-x-y-y-y------|';

    const result = e1.pipe(exhaustMap((x) => e2.pipe(map((i) => i * +x))));

    expectObservable(result).toBe(expected, values);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 46. 1 spawns an inner at 2; 3 arrives at 8, after it finished, and spawns the second; 5 arrives at 11 while that one runs and is dropped, its project never called (the cross on the operator row). No 50 group.

<!-- rxjs-visualizer:end -->
