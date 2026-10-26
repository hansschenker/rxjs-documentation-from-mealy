# `throttle` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`throttle` opens a suppression duration from an accepted source value and controls whether leading and trailing values are emitted.

With the default configuration, the first value is emitted immediately and starts a duration; source values arriving while that duration is active are ignored. The duration is not restarted by those values.

With trailing enabled, the latest value seen during the duration is remembered and may be emitted when the duration ends; that trailing emission can start the next throttle period. Source completion, selector behavior, and the leading/trailing configuration determine whether a remembered trailing value is emitted before completion. Errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `throttle` run frame by frame](https://netxpert.ch/rxjs-visualizer/throttle.html)**. The page runs the RxJS 7.8.2 marble test below, *should immediately emit the first value in each time window*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should immediately emit the first value in each time window', () => {
  testScheduler.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
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
    const expected = '-a--------b-----c----|';

    const result = e1.pipe(throttle(() => e2));

    expectObservable(result).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
    expectSubscriptions(e2.subscriptions).toBe(e2subs);
  });
});
```

Reserve list, throttle-spec.ts at tag 7.8.2, the first test. The duration selector returns the cold e2, so the spec is a higher-order one with the duration as its inner: one row per window, opened by a, b and c, closed when i fires at 4; the values landing inside a window are crossed out on the operator row, as exhaustMap's dropped value is.

<!-- rxjs-visualizer:end -->
