# `delay` Operator Behavior

## Operator Behavior in Plain Language

`delay` shifts source values forward in time by the configured amount.

Each source value is held until its scheduled delivery time and is then forwarded. Delayed values remain ordered according to their scheduled delivery times.

Normal completion waits for delayed values that still need to be delivered, then completes. Source errors terminate rather than being converted into ordinary delayed values. The scheduler supplies the clock and execution mechanism.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `delay` run frame by frame](https://netxpert.ch/rxjs-visualizer/delay.html)**. The page runs the RxJS 7.8.2 marble test below, *should delay by specified timeframe*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should delay by specified timeframe', () => {
  testScheduler.run(({ hot, time, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  ---a--b--|');
    const e1subs = '  ^--------!';
    const t = time('     --|    ');
    //                      --|
    const expected = '-----a--b|';

    const result = e1.pipe(delay(t));

    expectObservable(result).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 12. Each value is held for 2 frames; completion is not delayed because nothing is pending when the source completes at 9.

<!-- rxjs-visualizer:end -->
