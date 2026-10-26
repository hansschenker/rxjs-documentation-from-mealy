# `takeLast` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`takeLast` keeps only the final configured number of source values in a rolling buffer and emits nothing while the source is active.

As new values arrive, older values that can no longer belong to the final tail are discarded from the buffer.

When the source completes normally, the buffered tail is emitted in original source order and the result completes. If the source errors, the error is forwarded and the buffered values are not emitted.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `takeLast` run frame by frame](https://netxpert.ch/rxjs-visualizer/takeLast.html)**. The page runs the RxJS 7.8.2 marble test below, *should take two values of an observable with many values*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should take two values of an observable with many values', () => {
  rxTest.run(({ cold, expectObservable, expectSubscriptions }) => {
    const e1 = cold('--a-----b----c---d--|    ');
    const e1subs = ' ^-------------------!    ';
    const expected = '--------------------(cd|)';

    expectObservable(e1.pipe(takeLast(2))).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 25. Nothing leaves early: the last two values are held and emitted together at completion, c, d and | all on frame 20, drawn stepped right in that order.

<!-- rxjs-visualizer:end -->
