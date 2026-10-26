# `mergeMap` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`mergeMap` projects each source value to an inner Observable and allows projected inners to overlap.

When concurrency capacity is available, a source value is projected and its inner is subscribed immediately. If the configured concurrency limit is reached, later source values wait until an active inner completes.

Values from all active inners are forwarded as they arrive. The result completes only after the source is complete, all active inners are complete, and the queue is empty. Source, projection, or inner errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `mergeMap` run frame by frame](https://netxpert.ch/rxjs-visualizer/mergeMap.html)**. The page runs the RxJS 7.8.2 marble test below, *should map-and-flatten each item to an Observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should map-and-flatten each item to an Observable', () => {
  testScheduler.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const values = { x: 10, y: 30, z: 50 };
    const x = cold('    x-x-x|             ', values);
    //                        y-y-y|
    //                           z-z-z|
    const xsubs = [
      '               --^----!             ',
      '               --------^----!       ',
      '               -----------^----!    ',
    ];
    const e1 = hot('  --1-----3--5--------|');
    const e1subs = '  ^-------------------!';
    const expected = '--x-x-x-y-yzyz-z----|';

    const result = e1.pipe(mergeMap((value) => x.pipe(map((i) => i * +value))));

    expectObservable(result).toBe(expected, values);
    expectSubscriptions(x.subscriptions).toBe(xsubs);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

The project function is split from the flattening operator so the visualizer can tap each projected inner. `Number(i) * Number(value)` is the typed spelling of the spec's `i * +value`.

<!-- rxjs-visualizer:end -->
