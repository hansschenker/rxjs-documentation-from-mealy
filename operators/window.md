# `window` Operator Behavior

## Operator Behavior in Plain Language

`window` partitions source values into nested Observables instead of arrays.

It starts with one open window and emits that window from the outer result. Source values are forwarded into the current window.

Whenever the boundary Observable emits, the current window completes and a new window is opened and emitted. Source completion completes the current window and the outer result. Errors from the source or boundary Observable error the active window and outer result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `window` run frame by frame](https://netxpert.ch/rxjs-visualizer/window.html)**. The page runs the RxJS 7.8.2 marble test below, *should emit windows that close and reopen*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should emit windows that close and reopen', () => {
  rxTestScheduler.run(({ hot, cold, expectObservable, expectSubscriptions }) => {
    const source = hot('  ---a---b---c---d---e---f---g---h---i---|    ');
    const sourceSubs = '  ^--------------------------------------!    ';
    const closings = hot('-------------w------------w----------------|');
    const closingSubs = ' ^--------------------------------------!    ';
    const expected = '    x------------y------------z------------|    ';
    const x = cold('      ---a---b---c-|                              ');
    const y = cold('                   --d---e---f--|                 ');
    const z = cold('                                -g---h---i---|    ');
    const expectedValues = { x: x, y: y, z: z };

    const result = source.pipe(window(closings));

    expectObservable(result).toBe(expected, expectedValues);
    expectSubscriptions(source.subscriptions).toBe(sourceSubs);
    expectSubscriptions(closings.subscriptions).toBe(closingSubs);
  });
});
```

buffer's picture with windows instead of arrays: every value of the closings notifier ends one window and opens the next, so the output has a window at 0, 13 and 26; each is a row under the output lane. Both inputs are unsubscribed when the source completes at 39.

<!-- rxjs-visualizer:end -->
