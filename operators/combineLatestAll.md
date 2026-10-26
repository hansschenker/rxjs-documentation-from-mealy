# `combineLatestAll` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`combineLatestAll` receives inner Observables from an outer Observable and waits for the outer to complete before it starts combining them.

After every inner has emitted at least once, any new value from any inner emits a new array containing the latest value from every inner.

If an inner completes after producing a value, its last value remains part of later combinations. The result completes when all inner Observables complete. An outer or inner error terminates the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `combineLatestAll` run frame by frame](https://netxpert.ch/rxjs-visualizer/combineLatestAll.html)**. The page runs the RxJS 7.8.2 marble test below, *should combine events from two observables*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should combine events from two observables', () => {
  testScheduler.run(({ hot, cold, expectObservable }) => {
    const x = cold('                  -a-----b---|');
    const y = cold('                  --1-2-|     ');
    const outer = hot('-x----y--------|           ', { x: x, y: y });
    const expected = ' -----------------A-B--C---|';

    const result = outer.pipe(combineLatestAll((a, b) => String(a) + String(b)));

    expectObservable(result).toBe(expected, { A: 'a1', B: 'a2', C: 'b2' });
  });
});
```

The inners are collected until the outer completes at 15 and only then subscribed, both at once (their rows show the wait as queued spans), and combined as combineLatest would: the first pair at 17 once both have a value, then every change.

<!-- rxjs-visualizer:end -->
