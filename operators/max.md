# `max` Operator Behavior

## Operator Behavior in Plain Language

`max` remembers the greatest source value seen so far according to the supplied comparer, or normal JavaScript ordering when no comparer is supplied.

It emits nothing while the source is active because a later value may become the maximum.

When the source completes, the remembered maximum is emitted once and the result completes. With no source values and no seed concept, no maximum can be produced. Source or comparer errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `max` run frame by frame](https://netxpert.ch/rxjs-visualizer/max.html)**. The page runs the RxJS 7.8.2 marble test below, *should find the max of values of an observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should find the max of values of an observable', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --a--b--c--|   ', { a: 42, b: -1, c: 3 });
    const e1subs = '  ^----------!   ';
    const expected = '-----------(x|)';

    expectObservable(e1.pipe(max())).toBe(expected, { x: 42 });
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

<!-- rxjs-visualizer:end -->
