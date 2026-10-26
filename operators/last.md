# `last` Operator Behavior

## Operator Behavior in Plain Language

`last` remembers the most recent source value that qualifies, using the optional predicate when supplied.

It emits nothing while the source is running because a later qualifying value may replace the current candidate.

When the source completes, the remembered value is emitted and the result completes. If no qualifying value exists, the supplied default is emitted when present; otherwise the result errors with `EmptyError`. Source or predicate errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `last` run frame by frame](https://netxpert.ch/rxjs-visualizer/last.html)**. The page runs the RxJS 7.8.2 marble test below, *should take the last value of an observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should take the last value of an observable', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --a----b--c--|   ');
    const e1subs = '  ^------------!   ';
    const expected = '-------------(c|)';

    expectObservable(e1.pipe(last())).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 30. Each value replaces the remembered one; nothing is emitted until the source completes at 13, then c and the completion together.

<!-- rxjs-visualizer:end -->
