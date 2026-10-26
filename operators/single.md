# `single` Operator Behavior

## Operator Behavior in Plain Language

`single` verifies that exactly one source value satisfies the optional predicate.

A qualifying value is remembered, but it is not emitted immediately because a later qualifying value would violate the uniqueness requirement. If a second qualifying value arrives, `single` errors immediately.

When the source completes with exactly one qualifying value, that value is emitted and the result completes. If there is no qualifying value, the result errors. Source or predicate errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `single` run frame by frame](https://netxpert.ch/rxjs-visualizer/single.html)**. The page runs the RxJS 7.8.2 marble test below, *should raise error from empty predicate if observable emits multiple time*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should raise error from empty predicate if observable emits multiple time', () => {
  rxTest.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --a--b--c--|');
    const e1subs = '  ^----!      ';
    const expected = '-----#      ';

    expectObservable(e1.pipe(single())).toBe(expected, null, new SequenceError('Too many matching values'));
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Reserve list, single-spec.ts at tag 7.8.2, the first test. The second value is one too many: single() errors at 5 with a SequenceError and unsubscribes the source there.

<!-- rxjs-visualizer:end -->
