# `concatMap` Operator Behavior

## Operator Behavior in Plain Language

`concatMap` starts with no active inner stream and an empty queue.

When a source value arrives and no inner is active, the projection function creates an inner Observable and `concatMap` subscribes to it. Source values arriving while an inner is active wait in order rather than starting overlapping work.

Inner values are forwarded downstream. When an inner completes, the next queued source value is projected. If the source completes, queued work still finishes before the result completes. Source, projection, or inner errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `concatMap` run frame by frame](https://netxpert.ch/rxjs-visualizer/concatMap.html)**. The page runs the RxJS 7.8.2 marble test below, *should map-and-flatten each item to an Observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should map-and-flatten each item to an Observable', () => {
  testScheduler.run(({ hot, cold, expectObservable, expectSubscriptions }) => {
    const e1 = hot('   --1-----3--5-------|');
    const e1subs = '   ^------------------!';
    const e2 = cold('  x-x-x|              ', { x: 10 });
    const expected = ' --x-x-x-y-y-yz-z-z-|';
    const values = { x: 10, y: 30, z: 50 };

    const result = e1.pipe(concatMap((x) => e2.pipe(map((i) => i * parseInt(x)))));

    expectObservable(result).toBe(expected, values);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

The third outer value (5) arrives at frame 11 while the second inner is still running; concatMap queues it and subscribes its inner at 13.

A second page: **[concatMapMany](https://netxpert.ch/rxjs-visualizer/concatMapMany.html)**, *should concatMap many outer values to many inner values*.

<!-- rxjs-visualizer:end -->
