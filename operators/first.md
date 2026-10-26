# `first` Operator Behavior

## Operator Behavior in Plain Language

`first` looks for the first source value, or the first value satisfying an optional predicate.

As soon as a qualifying value arrives, it is emitted and the result completes immediately. Later source values are never needed.

If the source completes before a qualifying value is found, `first` emits the supplied default value when one was provided; otherwise it errors with `EmptyError`. Source or predicate errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `first` run frame by frame](https://netxpert.ch/rxjs-visualizer/first.html)**. The page runs the RxJS 7.8.2 marble test below, *should take the first value of an observable with many values*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should take the first value of an observable with many values', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  -----a--b--c---d---|');
    const e1subs = '  ^----!              ';
    const expected = '-----(a|)           ';

    expectObservable(e1.pipe(first())).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 29. The first value is emitted and the output completes in the same frame, unsubscribing at 5 before b, c, d.

<!-- rxjs-visualizer:end -->
