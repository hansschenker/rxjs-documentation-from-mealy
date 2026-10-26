# `map` Operator Behavior

## Operator Behavior in Plain Language

`map` applies a projection function to every source value independently.

Each source value is passed to the projection together with its zero-based index, and the returned value is emitted immediately downstream.

`map` changes the value, not the timing or cardinality of successful source emissions. Source completion and errors are forwarded; an error thrown by the projection terminates the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `map` run frame by frame](https://netxpert.ch/rxjs-visualizer/map.html)**. The page runs the RxJS 7.8.2 marble test below, *should map multiple values*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should map multiple values', () => {
  testScheduler.run(({ cold, expectObservable, expectSubscriptions }) => {
    const e1 = cold(' --1--2--3--|');
    const e1subs = '  ^----------!';
    const expected = '--x--y--z--|';

    const result = e1.pipe(map((x) => 10 * +x));

    expectObservable(result).toBe(expected, { x: 10, y: 20, z: 30 });
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 06 of the document; the first of two tests with this title in map-spec.ts. A cold source: its sequence starts at the subscription. `10 * Number(x)` is the typed spelling of `10 * +x`.

<!-- rxjs-visualizer:end -->
