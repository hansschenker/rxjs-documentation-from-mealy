# `zipAll` Operator Behavior

## Operator Behavior in Plain Language

`zipAll` receives inner Observables from an outer Observable and waits for the outer to complete before zipping those inners together.

After subscription to the collected inners, each inner has its own queue. A combined value is emitted only when every inner has an available value, consuming one value from each queue.

The result completes when no further full tuple can be formed, such as when a completed inner has no queued value left. Outer or inner errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `zipAll` run frame by frame](https://netxpert.ch/rxjs-visualizer/zipAll.html)**. The page runs the RxJS 7.8.2 marble test below, *should combine paired events from two observables*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should combine paired events from two observables', () => {
  rxTestScheduler.run(({ hot, cold, expectObservable }) => {
    const x = cold('                  -a-----b-|');
    const y = cold('                  --1-2-----');
    const outer = hot('-x----y--------|         ', { x: x, y: y });
    const expected = ' -----------------A----B-|';

    const result = outer.pipe(zipAll((a, b) => a + b));

    expectObservable(result).toBe(expected, { A: 'a1', B: 'b2' });
  });
});
```

Like combineLatestAll the inners wait for the outer to complete at 15; then they are zipped: a with 1 at 17, b with 2 at 22, and the output completes at 24 with x, the 2 of y having been consumed. y never completes.

<!-- rxjs-visualizer:end -->
