# `switchAll` Operator Behavior

## Operator Behavior in Plain Language

`switchAll` keeps at most one inner Observable active: the most recently emitted inner from the outer Observable.

The first inner is subscribed. Whenever a newer inner arrives, the current inner subscription is cancelled and the new inner becomes active.

Only values from the current inner are forwarded. If the outer completes while an inner remains active, that inner is allowed to continue; the result completes when both the outer has completed and no active inner remains. Outer or active-inner errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `switchAll` run frame by frame](https://netxpert.ch/rxjs-visualizer/switchAll.html)**. The page runs the RxJS 7.8.2 marble test below, *should switch a hot observable of cold observables*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should switch a hot observable of cold observables', () => {
  testScheduler.run(({ hot, cold, expectObservable, expectSubscriptions }) => {
    const x = cold('    --a---b--c---d--|      ');
    const xsubs = '   --^------!               ';
    const y = cold('           ----e---f--g---|');
    const ysubs = '   ---------^--------------!';
    const e1 = hot('  --x------y-------|       ', { x: x, y: y });
    const e1subs = '  ^----------------!       ';
    const expected = '----a---b----e---f--g---|';

    const result = e1.pipe(switchAll());

    expectObservable(result).toBe(expected);
    expectSubscriptions(x.subscriptions).toBe(xsubs);
    expectSubscriptions(y.subscriptions).toBe(ysubs);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 44. The same inputs as mergeAll, a different subscription trace: y's arrival at 9 unsubscribes x there, so c and d stay on x's row past its bar and never reach the output.

<!-- rxjs-visualizer:end -->
