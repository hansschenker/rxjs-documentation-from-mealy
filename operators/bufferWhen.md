# `bufferWhen` Operator Behavior

## Operator Behavior in Plain Language

`bufferWhen` starts by opening an empty buffer and creating a closing Observable from the closing selector.

Source values are collected into the current buffer. When the current closing Observable emits or completes, `bufferWhen` emits that buffer, opens a new empty buffer, and calls the selector again for the next closing Observable.

The cycle repeats until the source completes. On source completion, the current buffer is emitted and the result completes. Source, selector, or closing-stream errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `bufferWhen` run frame by frame](https://netxpert.ch/rxjs-visualizer/bufferWhen.html)**. The page runs the RxJS 7.8.2 marble test below, *should emit buffers that close and reopen*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should emit buffers that close and reopen', () => {
  testScheduler.run(({ hot, cold, expectObservable }) => {
    const e1 = hot('--a--^---b---c---d---e---f---g---------|   ');
    const e2 = cold('    --------------(s|)                    ');
    //                                 --------------(s|)
    const expected = '   --------------x-------------y-----(z|)';
    const values = {
      x: ['b', 'c', 'd'],
      y: ['e', 'f', 'g'],
      z: [] as string[],
    };

    expectObservable(e1.pipe(bufferWhen(() => e2))).toBe(expected, values);
  });
});
```

The closing selector is called at subscription and again every time a buffer closes, so the cold closing is subscribed at 0, 14 and 28; each s emits the buffer and opens the next. The source completes at 34 while the third buffer is open and empty: it is emitted as [] with the completion.

<!-- rxjs-visualizer:end -->
