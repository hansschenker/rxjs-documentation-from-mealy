# `skipUntil` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`skipUntil` starts with its gate closed and subscribes to a notifier as well as the source.

Source values are suppressed while the gate is closed. The first notifier `next` opens the gate permanently, after which all later source values are forwarded.

If the notifier completes without emitting, the gate remains closed and source values continue to be skipped until the source itself completes. Source or notifier errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `skipUntil` run frame by frame](https://netxpert.ch/rxjs-visualizer/skipUntil.html)**. The page runs the RxJS 7.8.2 marble test below, *should skip values until another observable notifies*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should skip values until another observable notifies', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('    --a--b--c--d--e----|');
    const e1subs = '    ^------------------!';
    const skip = hot('  ---------x------|   ');
    const skipSubs = '  ^--------!          ';
    const expected = '  -----------d--e----|';

    expectObservable(e1.pipe(skipUntil(skip))).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
    expectSubscriptions(skip.subscriptions).toBe(skipSubs);
  });
});
```

Entry 16. x opens the gate at 9 and the notifier is dropped right there; a, b, c before it are skipped, d and e pass, completion follows the source.

<!-- rxjs-visualizer:end -->
