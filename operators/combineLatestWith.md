# `combineLatestWith` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`combineLatestWith` subscribes to the source and the supplied Observables and remembers the latest value from each one.

It emits nothing until every participant has produced at least one value. Once all are ready, any new emission from any participant produces an array of the current latest values.

A completed participant can continue contributing its last value. The result completes after all participants complete, and an error from any participant terminates the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `combineLatestWith` run frame by frame](https://netxpert.ch/rxjs-visualizer/combineLatestWith.html)**. The page runs the RxJS 7.8.2 marble test below, *should combine events from two cold observables*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should combine events from two cold observables', () => {
  testScheduler.run(({ cold, expectObservable }) => {
    const e1 = cold(' -a--b-----c-d-e-|');
    const e2 = cold(' --1--2-3-4---|   ');
    const expected = '--A-BC-D-EF-G-H-|';

    const result = e1.pipe(
      combineLatestWith(e2),
      map(([a, b]) => a + b)
    );

    expectObservable(result).toBe(expected, {
      A: 'a1',
      B: 'b1',
      C: 'b2',
      D: 'b3',
      E: 'b4',
      F: 'c4',
      G: 'd4',
      H: 'e4',
    });
  });
});
```

Entry 18. Nothing until both inputs have a value (A at 2), then every value of either input re-emits the latest pair. e2 completing at 13 changes nothing: its last value 4 keeps being combined. `String(a) + String(b)` is the typed spelling of `a + b`.

<!-- rxjs-visualizer:end -->
