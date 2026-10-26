# `skipLast` Operator Behavior

## Operator Behavior in Plain Language

`skipLast` keeps a rolling buffer large enough to withhold the configured number of most recent source values.

Before the buffer exceeds that size, nothing is emitted. Once it is full, each new source value causes the oldest buffered value to be emitted while the new value becomes part of the withheld tail.

When the source completes, the final configured number of buffered values are discarded and the result completes. A source error terminates immediately without flushing that tail.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `skipLast` run frame by frame](https://netxpert.ch/rxjs-visualizer/skipLast.html)**. The page runs the RxJS 7.8.2 marble test below, *should skip two values of an observable with many values*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should skip two values of an observable with many values', () => {
  testScheduler.run(({ cold, expectObservable, expectSubscriptions }) => {
    const e1 = cold(' --a-----b----c---d--|');
    const e1subs = '  ^-------------------!';
    const expected = '-------------a---b--|';

    expectObservable(e1.pipe(skipLast(2))).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

takeLast's mirror image: the operator holds the last two values back, so a is only emitted at 13 when c arrives (c might have been one of the last two), b at 17 when d arrives, and c and d never, since the source completes at 20 with them held.

<!-- rxjs-visualizer:end -->
