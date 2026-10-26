# `isEmpty` Operator Behavior

## Operator Behavior in Plain Language

`isEmpty` determines whether the source emits any value.

If the first source value arrives, `isEmpty` emits `false` and completes immediately because the answer is known.

If the source completes before any value arrives, it emits `true` and completes. A source error is forwarded without producing a Boolean result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `isEmpty` run frame by frame](https://netxpert.ch/rxjs-visualizer/isEmpty.html)**. The page runs the RxJS 7.8.2 marble test below, *should return false if source emits element*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should return false if source emits element', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('--a--^--b--|');
    const e1subs = '     ^--!   ';
    const expected = '   ---(F|)';

    expectObservable(e1.pipe(isEmpty())).toBe(expected, { F: false });
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

The a before the `^` was emitted before anyone subscribed and does not count; b at 3 answers the question, so false is emitted there with the completion and the source is unsubscribed.

<!-- rxjs-visualizer:end -->
