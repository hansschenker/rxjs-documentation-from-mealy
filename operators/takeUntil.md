# `takeUntil` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`takeUntil` forwards source values while waiting for a notifier to emit.

If the notifier emits a value, `takeUntil` completes the result immediately and unsubscribes from the source; the notifier value itself is not emitted.

If the notifier merely completes without emitting, it does not stop the source and source values continue normally. Source completion completes the result, while an error from either source or notifier terminates it with an error.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `takeUntil` run frame by frame](https://netxpert.ch/rxjs-visualizer/takeUntil.html)**. The page runs the RxJS 7.8.2 marble test below, *should take values until notifier emits*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should take values until notifier emits', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --a--b--c--d--e--f--g--|');
    const e1subs = '  ^------------!          ';
    const e2 = hot('  -------------z--|       ');
    const e2subs = '  ^------------!          ';
    const expected = '--a--b--c--d-|          ';

    expectObservable(e1.pipe(takeUntil(e2))).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
    expectSubscriptions(e2.subscriptions).toBe(e2subs);
  });
});
```

Entry 15. z on the notifier completes the output at 13 and unsubscribes both inputs there: e, f, g never pass, and z itself is not forwarded.

<!-- rxjs-visualizer:end -->
