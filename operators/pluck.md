# `pluck` Operator Behavior

## Operator Behavior in Plain Language

`pluck` reads a configured property path from every source value and emits the value found at that path.

Each source value produces one downstream value. If an intermediate property is missing, the extracted result is `undefined` rather than the original object.

Source completion and errors are forwarded. `pluck` is deprecated in favor of `map` with normal property access.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `pluck` run frame by frame](https://netxpert.ch/rxjs-visualizer/pluck.html)**. The page runs the RxJS 7.8.2 marble test below, *should work for one object*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should work for one object', () => {
  testScheduler.run(({ cold, expectObservable, expectSubscriptions }) => {
    const inputs = { x: { prop: 42 } };
    const e1 = cold(' --x--|', inputs);
    const e1subs = '  ^----!';
    const expected = '--y--|';

    const result = e1.pipe(pluck('prop'));

    expectObservable(result).toBe(expected, { y: 42 });
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

<!-- rxjs-visualizer:end -->
