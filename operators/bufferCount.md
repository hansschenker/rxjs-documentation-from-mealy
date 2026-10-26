# `bufferCount` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`bufferCount` groups source values into arrays according to a maximum buffer size and an optional interval for starting new buffers.

With only a buffer size, one buffer is active at a time and is emitted when it reaches that size. With a start interval, new buffers open every configured number of source values, so buffers may overlap or leave gaps.

Each buffer is emitted when full. When the source completes, any non-empty buffers still open are emitted before completion. A source error terminates without flushing incomplete buffers.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `bufferCount` run frame by frame](https://netxpert.ch/rxjs-visualizer/bufferCount.html)**. The page runs the RxJS 7.8.2 marble test below, *should emit full buffer then last partial buffer if source completes*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should emit full buffer then last partial buffer if source completes', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --a^-b--c--d--e--|');
    const e1subs = '     ^-------------!';
    const expected = '   --------y-----(z|)';

    expectObservable(e1.pipe(bufferCount(3))).toBe(expected, { y: ['b', 'c', 'd'], z: ['e'] });
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 34. a sits before the `^`, at frame -1, outside the subscription. b, c, d fill the first buffer at 8; e is flushed alone when the source completes at 14.

<!-- rxjs-visualizer:end -->
