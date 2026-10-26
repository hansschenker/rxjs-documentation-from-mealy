# `bufferToggle` Operator Behavior

## Operator Behavior in Plain Language

`bufferToggle` can keep several buffers open at once.

Each emission from the openings Observable creates a new empty buffer. The closing selector is called for that opening value, and the returned Observable controls only that buffer.

Every source value is copied into every currently open buffer. When a buffer's closing Observable emits or completes, that buffer is emitted and removed.

Source completion flushes all open buffers and completes. Errors from the source, openings stream, selector, or closing streams terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `bufferToggle` run frame by frame](https://netxpert.ch/rxjs-visualizer/bufferToggle.html)**. The page runs the RxJS 7.8.2 marble test below, *should emit buffers using hot openings and hot closings*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should emit buffers using hot openings and hot closings', () => {
  testScheduler.run(({ hot, expectObservable }) => {
    const e1 = hot('  ---a---b---c---d---e---f---g---|');
    const e2 = hot('  --o------------------o---------|');
    const e3 = hot('  ---------c---------------c-----|');
    const expected = '---------x---------------y-----|';
    const values = {
      x: ['a', 'b'],
      y: ['f'],
    };

    const result = e1.pipe(bufferToggle(e2, (x: any) => e3));

    expectObservable(result).toBe(expected, values);
  });
});
```

Each o of the openings e2 opens a buffer and subscribes the closing e3; the next c of e3 emits the buffer and ends that subscription. The values between openings, c, d and e, belong to no buffer and are lost.

<!-- rxjs-visualizer:end -->
