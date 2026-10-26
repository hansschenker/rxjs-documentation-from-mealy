# `ignoreElements` Operator Behavior

## Operator Behavior in Plain Language

`ignoreElements` suppresses every source `next` value.

It does not suppress lifecycle signals: source completion is forwarded as completion and a source error is forwarded as an error.

The result therefore carries only the source's terminal behavior, never its ordinary values.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `ignoreElements` run frame by frame](https://netxpert.ch/rxjs-visualizer/ignoreElements.html)**. The page runs the RxJS 7.8.2 marble test below, *should ignore all the elements of the source*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should ignore all the elements of the source', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --a--b--c--d--|');
    const e1subs = '  ^-------------!';
    const expected = '--------------|';

    expectObservable(e1.pipe(ignoreElements())).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

<!-- rxjs-visualizer:end -->
