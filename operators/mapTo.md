# `mapTo` Operator Behavior

## Operator Behavior in Plain Language

`mapTo` replaces every source value with the same configured constant value.

Each source `next` therefore produces exactly one downstream `next`, but the original payload is ignored.

Source completion and errors are forwarded unchanged. `mapTo` is deprecated in favor of `map(() => constant)`.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `mapTo` run frame by frame](https://netxpert.ch/rxjs-visualizer/mapTo.html)**. The page runs the RxJS 7.8.2 marble test below, *should map multiple values*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should map multiple values', () => {
  testScheduler.run(({ cold, expectObservable, expectSubscriptions }) => {
    const e1 = cold(' --1--2--3--|');
    const e1subs = '  ^----------!';
    const expected = '--a--a--a--|';

    expectObservable(e1.pipe(mapTo('a'))).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

<!-- rxjs-visualizer:end -->
