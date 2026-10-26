# `sampleTime` Operator Behavior

## Operator Behavior in Plain Language

`sampleTime` periodically checks for the latest source value using the configured scheduler and period.

Source emissions update the remembered latest value but do not emit immediately. At each sampling tick, the latest value is emitted only if at least one new source value has arrived since the previous sample.

The result completes when the source completes. Source errors terminate immediately. The scheduler supplies the periodic clock.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `sampleTime` run frame by frame](https://netxpert.ch/rxjs-visualizer/sampleTime.html)**. The page runs the RxJS 7.8.2 marble test below, *should get samples on a delay*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should get samples on a delay', () => {
  rxTest.run(({ hot, expectObservable, expectSubscriptions, time }) => {
    const e1 = hot('     a---b-c---------d--e---f-g-h--|');
    const e1subs = '     ^-----------------------------!';
    const expected = '   -------c-------------e------h-|';
    // period            -------!------!------!------!--
    const period = time('-------|                       ');

    expectObservable(e1.pipe(sampleTime(period, rxTest))).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 11. The clock runs from subscription, not from a value: the tick at 14 finds nothing new since 7 and emits nothing, and completion at 30 does not flush anything either.

<!-- rxjs-visualizer:end -->
