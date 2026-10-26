# `elementAt` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`elementAt` counts source values from index zero and emits nothing before the requested position.

When the requested index arrives, that value is emitted and the result completes immediately because no later value can change the answer.

If the source completes before reaching the requested index, the supplied default value is emitted when one exists; otherwise the result errors with `ArgumentOutOfRangeError`. A source error is forwarded.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `elementAt` run frame by frame](https://netxpert.ch/rxjs-visualizer/elementAt.html)**. The page runs the RxJS 7.8.2 marble test below, *should return next to last element by zero-based index*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should return next to last element by zero-based index', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --a--b--c-d---|');
    const e1subs = '  ^-------!      ';
    const expected = '--------(c|)   ';

    expectObservable(e1.pipe(elementAt(2))).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Reserve list, elementAt-spec.ts at tag 7.8.2, the first test. Index 2 is the third value: c at 8 is emitted and the output completes there, before d and the source's own completion.

<!-- rxjs-visualizer:end -->
