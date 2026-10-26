# `auditTime` Operator Behavior

## Operator Behavior in Plain Language

`auditTime` starts with no audit window active.

When a source value arrives while no window is active, it remembers that value and starts the configured duration on the scheduler. Later values during that duration replace the remembered value without restarting the timer.

When the duration ends, `auditTime` emits the latest remembered value and becomes ready for another window.

If the source completes while a value is pending, the pending value is emitted when the current duration ends and the result then completes. Source errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `auditTime` run frame by frame](https://netxpert.ch/rxjs-visualizer/auditTime.html)**. The page runs the RxJS 7.8.2 marble test below, *should emit the last value in each time window*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should emit the last value in each time window', () => {
  testScheduler.run(({ hot, time, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  -a-x-y----b---x-cx---|');
    const e1subs = '  ^--------------------!';
    const t = time('   -----|               ');
    //                          -----|
    //                                -----|
    const expected = '------y--------x-----(x|)';
    const result = e1.pipe(auditTime(t));
    expectObservable(result).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

The last window ends on the frame the source completes; auditTime emits the pending value first and then completes, hence the `(x|)` group.

<!-- rxjs-visualizer:end -->
