# `take` Operator Behavior

## Operator Behavior in Plain Language

`take` forwards only the first configured number of source values.

Each source value is counted and emitted while the count remains within the requested amount. As soon as the requested number of values has been emitted, `take` completes immediately and unsubscribes from the source.

If the source completes earlier, the result simply completes with however many values were available. A non-positive count produces an empty result without needing source values. Source errors before completion are forwarded.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `take` run frame by frame](https://netxpert.ch/rxjs-visualizer/take.html)**. The page runs the RxJS 7.8.2 marble test below, *should take two values of an observable with many values*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should take two values of an observable with many values', () => {
  testScheduler.run(({ cold, expectObservable, expectSubscriptions }) => {
    const e1 = cold(' --a-----b----c---d--|');
    const e1subs = '  ^-------!------------';
    const expected = '--a-----(b|)         ';

    expectObservable(e1.pipe(take(2))).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 08. take(2) completes in the frame of the second value, `(b|)`, and unsubscribes there: c, d and the source's own `|` stay on the lane after the subscription bar ends.

<!-- rxjs-visualizer:end -->
