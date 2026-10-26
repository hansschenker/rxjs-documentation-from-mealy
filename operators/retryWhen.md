# `retryWhen` Operator Behavior

## Operator Behavior in Plain Language

`retryWhen` turns source errors into signals that can decide whether another source subscription should start.

Source values are forwarded normally. When the source errors, that error is sent to the errors Observable supplied to the user-defined notifier function instead of immediately terminating downstream.

Each notifier emission causes a fresh subscription to the source. If the notifier completes, the result completes; if it errors, the result errors. A normal source completion completes normally and does not invoke retry behavior.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `retryWhen` run frame by frame](https://netxpert.ch/rxjs-visualizer/retryWhen.html)**. The page runs the RxJS 7.8.2 marble test below, *should handle a source with eventual error using a hot notifier*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should handle a source with eventual error using a hot notifier', () => {
  rxTest.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const source = cold(' -1--2--#                     ');
    //                                 -1--2--#
    //                                              -1--2--#
    const subs = [
      '                   ^------!                     ',
      '                   -------------^------!        ',
      '                   --------------------------^-!',
    ];
    const notifier = hot('-------------r------------r-|');
    const expected = '    -1--2---------1--2---------1|';

    const result = source.pipe(retryWhen(() => notifier));

    expectObservable(result).toBe(expected);
    expectSubscriptions(source.subscriptions).toBe(subs);
  });
});
```

retry's attempt rows, with the resubscription decided by a notifier: the cold errors at 7 and waits; each r of the notifier, at 13 and 26, subscribes it again. The notifier completes at 28 during the third run, which ends that run and completes the output: 1 at 27 is its only value.

<!-- rxjs-visualizer:end -->
