# `toArray` Operator Behavior

## Operator Behavior in Plain Language

`toArray` collects every source value into one array and emits nothing while the source is active.

Each incoming value is appended to the internal collection in source order.

When the source completes normally, the complete array is emitted once and the result completes. An empty source therefore emits an empty array. If the source errors, the error is forwarded and no accumulated array is emitted.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `toArray` run frame by frame](https://netxpert.ch/rxjs-visualizer/toArray.html)**. The page runs the RxJS 7.8.2 marble test below, *should reduce the values of an observable into an array*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should reduce the values of an observable into an array', () => {
  rxTestScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  ---a--b--|   ');
    const e1subs = '  ^--------!   ';
    const expected = '---------(w|)';

    expectObservable(e1.pipe(toArray())).toBe(expected, { w: ['a', 'b'] });
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 35. Every value is kept; one array leaves with the completion.

<!-- rxjs-visualizer:end -->
