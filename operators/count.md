# `count` Operator Behavior

## Operator Behavior in Plain Language

`count` starts at zero and emits nothing while the source is running.

For each source value, it increments the count directly or, when a predicate is supplied, only when the predicate returns true.

When the source completes, `count` emits the final count exactly once and then completes. A source error or predicate error terminates the result without emitting a final count.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `count` run frame by frame](https://netxpert.ch/rxjs-visualizer/count.html)**. The page runs the RxJS 7.8.2 marble test below, *should count the values of an observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should count the values of an observable', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const source = hot('--a--b--c--|');
    const subs = '      ^----------!';
    const expected = '  -----------(x|)';

    expectObservable(source.pipe(count())).toBe(expected, { x: 3 });
    expectSubscriptions(source.subscriptions).toBe(subs);
  });
});
```

Reserve list, count-spec.ts at tag 7.8.2, the first test. Nothing is emitted until the source completes; then the count and the completion share frame 11.

<!-- rxjs-visualizer:end -->
