# `exhaust` Operator Behavior

## Operator Behavior in Plain Language

`exhaust` receives inner Observables and allows only the first inner that arrives while no inner is active.

While that inner is active, every additional inner emitted by the outer Observable is ignored completely; there is no queue and no cancellation of the active inner.

When the active inner completes, the next inner that arrives may be accepted. If the outer completes while an inner is active, the result waits for that inner to finish. Outer or active-inner errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `exhaustAll` run frame by frame](https://netxpert.ch/rxjs-visualizer/exhaustAll.html)** (`exhaust` is a deprecated alias of `exhaustAll`, so its visualization is `exhaustAll`'s). The page runs the RxJS 7.8.2 marble test below, *should handle a hot observable of observables*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should handle a hot observable of observables', () => {
  testScheduler.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const x = cold('        --a---b---c--|               ');
    const xsubs = '   ------^------------!               ';
    const y = cold('                ---d--e---f---|      ');
    const ysubs: string[] = [];
    const z = cold('                      ---g--h---i---|');
    const zsubs = '   --------------------^-------------!';
    const e1 = hot('  ------x-------y-----z-------------|', { x: x, y: y, z: z });
    const e1subs = '  ^---------------------------------!';
    const expected = '--------a---b---c------g--h---i---|';

    expectObservable(e1.pipe(exhaustAll())).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
    expectSubscriptions(x.subscriptions).toBe(xsubs);
    expectSubscriptions(y.subscriptions).toBe(ysubs);
    expectSubscriptions(z.subscriptions).toBe(zsubs);
  });
});
```

exhaustMap's rule on a source of observables: y arrives at 14 while x still runs and is never subscribed (its row is missing, the value is crossed out on the operator row); z arrives at 20 after x completed at 19 and runs to its end.

<!-- rxjs-visualizer:end -->
