# `bufferTime` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`bufferTime` collects source values into arrays whose opening and closing are controlled by time.

A buffer remains active for the configured span, and every source value arriving while it is open is added to it. With a creation interval, several buffers can overlap; with a maximum size, a buffer can close early.

When a buffer closes, it is emitted as an array. On source completion, all active buffers are emitted and the result completes. Timing is provided by the configured scheduler; source errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `bufferTime` run frame by frame](https://netxpert.ch/rxjs-visualizer/bufferTime.html)**. The page runs the RxJS 7.8.2 marble test below, *should emit buffers at intervals*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should emit buffers at intervals', () => {
  testScheduler.run(({ hot, time, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  ---a---b---c---d---e---f---g-----|   ');
    const subs = '    ^--------------------------------!   ';
    const t = time('  ----------|                          ');
    const expected = '----------w---------x---------y--(z|)';
    const values = {
      w: ['a', 'b'],
      x: ['c', 'd', 'e'],
      y: ['f', 'g'],
      z: [] as string[],
    };

    const result = e1.pipe(bufferTime(t, null, Infinity, testScheduler));

    expectObservable(result).toBe(expected, values);
    expectSubscriptions(e1.subscriptions).toBe(subs);
  });
});
```

Entry 14. Consecutive 10-frame buffers from subscription; completion at 33 flushes the open, empty one as `[]` in the same frame, hence `(z|)`. Array values are drawn as `[a,b]` pills.

<!-- rxjs-visualizer:end -->
