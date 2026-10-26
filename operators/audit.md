# `audit` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`audit` starts without an active duration window.

When a source value arrives while no duration is active, `audit` remembers that value and creates a duration Observable from it. While that duration is active, later source values replace the remembered value but do not restart the duration.

When the duration emits or completes, `audit` emits the most recently remembered source value and becomes ready for a new duration window.

If the source completes while a value is pending, `audit` waits for the active duration to end, emits the pending value, and then completes. Source, selector, or duration errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `audit` run frame by frame](https://netxpert.ch/rxjs-visualizer/audit.html)**. The page runs the RxJS 7.8.2 marble test below, *should emit the last value in each time window*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should emit the last value in each time window', () => {
  testScheduler.run(({ hot, cold, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  -a-xy-----b--x--cxyz-|');
    const e1subs = '  ^--------------------!';
    const e2 = cold('  ----i                ');
    //                          ----i
    //                                ----i
    const e2subs = [
      '               -^---!                ',
      '               ----------^---!       ',
      '               ----------------^---! ',
    ];
    const expected = '-----y--------x-----z|';

    const result = e1.pipe(audit(() => e2));

    expectObservable(result).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
    expectSubscriptions(e2.subscriptions).toBe(e2subs);
  });
});
```

audit-spec.ts at tag 7.8.2, the first test: the same source and the same duration cold as the throttle spec, so the two read side by side. The duration selector returns the cold e2, so the spec is a higher-order one with the duration as its inner: one row per window, opened by a, b and c and closed when i fires at 4. Unlike throttle, the values landing inside a window are not dropped but folded in, and the last of them is emitted when the window closes (y at 5, x at 14, z at 20); `unprojected: 'folded'` makes the operator row tick them and name them as folded in.

<!-- rxjs-visualizer:end -->
