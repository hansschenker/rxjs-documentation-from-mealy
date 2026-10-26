# `onErrorResumeNext` Operator Behavior

## Operator Behavior in Plain Language

`onErrorResumeNext` forwards values from the source and then continues through the supplied Observable inputs in sequence.

When the current Observable completes normally, it moves to the next one. When the current Observable errors, that error is suppressed and it also moves to the next one instead of terminating.

The result completes after every supplied Observable has completed or errored. It is therefore sequential continuation that treats errors as a signal to continue, not as terminal output.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `onErrorResumeNext` run frame by frame](https://netxpert.ch/rxjs-visualizer/onErrorResumeNext.html)**. The page runs the RxJS 7.8.2 marble test below, *should continue observable sequence with next observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should continue observable sequence with next observable', () => {
  testScheduler.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --a--b--#        ');
    const e1subs = '  ^-------!        ';
    const e2 = cold('         --c--d--|');
    const e2subs = '  --------^-------!';
    const expected = '--a--b----c--d--|';

    expectObservable(e1.pipe(onErrorResumeNext(e2))).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
    expectSubscriptions(e2.subscriptions).toBe(e2subs);
  });
});
```

catchError's picture without the error: the source's `#` at 8 is swallowed and e2 is subscribed there, its marbles shifted to 8, and the output completes with e2. (A source completion would hand over the same way.)

<!-- rxjs-visualizer:end -->
