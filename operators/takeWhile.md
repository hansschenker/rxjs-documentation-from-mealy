# `takeWhile` Operator Behavior

## Operator Behavior in Plain Language

`takeWhile` forwards source values while a predicate remains true.

Each source value is tested in order. When the predicate first returns false, the result completes immediately and no later source values are observed.

By default the failing value is suppressed. With the inclusive option enabled, that first failing value is emitted once before completion. Source or predicate errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `takeWhile` run frame by frame](https://netxpert.ch/rxjs-visualizer/takeWhile.html)**. The page runs the RxJS 7.8.2 marble test below, *should take all elements until predicate is false*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should take all elements until predicate is false', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('-1-^2--3--4--5--6--|');
    const e1subs = '   ^------!         ';
    const expected = ' -2--3--|         ';

    const result = e1.pipe(takeWhile((v) => +v < 4));

    expectObservable(result).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 26. The `^` inside the hot string is the subscription: the 1 before it sits at frame -2, left of the timeline's 0, and is never seen. 4 fails the predicate at 7, completes the output there and is not emitted. `Number(v) < 4` is the typed spelling of `+v < 4`.

<!-- rxjs-visualizer:end -->
