# `buffer` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`buffer` starts with an empty buffer and listens to both the source and a closing notifier.

Each source value is added to the current buffer instead of being emitted immediately. Whenever the closing notifier emits, `buffer` emits the current buffer as an array and immediately starts a new empty buffer.

When the source completes, the current buffer is emitted and the result completes. A source or notifier error terminates the result; completion of the notifier alone does not complete the source-driven result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `buffer` run frame by frame](https://netxpert.ch/rxjs-visualizer/buffer.html)**. The page runs the RxJS 7.8.2 marble test below, *should emit buffers that close and reopen*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should emit buffers that close and reopen', () => {
  testScheduler.run(({ hot, expectObservable }) => {
    const a = hot('   -a-b-c-d-e-f-g-h-i-|');
    const b = hot('   -----B-----B-----B-|');
    const expected = '-----x-----y-----z-(F|)';
    const expectedValues = {
      x: ['a', 'b', 'c'],
      y: ['d', 'e', 'f'],
      z: ['g', 'h', 'i'],
      F: [],
    };
    expectObservable(a.pipe(buffer(b))).toBe(expected, expectedValues);
  });
});
```

Entry 17. The spec asserts no subscriptions, so the lanes draw theirs from the logs alone: both end at 19, when a completes and the open, empty buffer is flushed as `[]`. Each B closes a buffer on its frame.

<!-- rxjs-visualizer:end -->
