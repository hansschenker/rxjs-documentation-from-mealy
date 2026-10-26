# `filter` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`filter` examines each source value independently with a predicate.

If the predicate returns true, the original value is forwarded unchanged. If it returns false, the value is suppressed. The predicate receives the source value and its zero-based source index.

`filter` keeps no business state beyond that index. Source completion and errors are forwarded; an error thrown by the predicate terminates the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `filter` run frame by frame](https://netxpert.ch/rxjs-visualizer/filter.html)**. The page runs the RxJS 7.8.2 marble test below, *should filter out even values*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should filter out even values', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --0--1--2--3--4--|');
    const e1subs = '  ^----------------!';
    const expected = '-----1-----3-----|';

    expectObservable(e1.pipe(filter(oddFilter))).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 07. `oddFilter` is the spec file's shared helper, defined above.

<!-- rxjs-visualizer:end -->
