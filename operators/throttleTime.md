# `throttleTime` Operator Behavior

## Operator Behavior in Plain Language

`throttleTime` applies throttling with a fixed time duration supplied by the scheduler.

With the default configuration, the first source value is emitted immediately and starts a time window during which later source values are suppressed. Those suppressed values do not restart the window.

With trailing enabled, the latest suppressed value can be emitted when the window closes and can begin the next window. Source errors terminate immediately; completion follows the configured leading/trailing policy for any remembered value.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `throttleTime` run frame by frame](https://netxpert.ch/rxjs-visualizer/throttleTime.html)**. The page runs the RxJS 7.8.2 marble test below, *should immediately emit the first value in each time window*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should immediately emit the first value in each time window', () => {
  rxTest.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  -a-x-y----b---x-cx---|');
    //                 ----|    ----| ----|
    const expected = '-a--------b-----c----|';
    const subs = '    ^--------------------!';

    const result = e1.pipe(throttleTime(5));

    expectObservable(result).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(subs);
  });
});
```

Entry 10. The spec passes the literal 5; `time('-----|')` is 5 inside `run()`, so the duration reads the same. Leading only: a, b and c are emitted the moment they open a window, the x and y inside one are dropped.

<!-- rxjs-visualizer:end -->
