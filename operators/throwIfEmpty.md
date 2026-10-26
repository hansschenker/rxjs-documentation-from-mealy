# `throwIfEmpty` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`throwIfEmpty` forwards every source value unchanged while remembering whether at least one value has been seen.

If the source completes after emitting a value, the result completes normally.

If the source completes without any value, `throwIfEmpty` creates an error using the supplied factory, or an `EmptyError` by default, and terminates with that error. Source errors are forwarded directly.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `throwIfEmpty` run frame by frame](https://netxpert.ch/rxjs-visualizer/throwIfEmpty.html)**. The page runs the RxJS 7.8.2 marble test below, *should error when empty*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should error when empty', () => {
  rxTestScheduler.run(({ cold, expectObservable, expectSubscriptions }) => {
    const source = cold('----|');
    const sub1 = '       ^---!';
    const expected = '   ----#';

    const result = source.pipe(throwIfEmpty());

    expectObservable(result).toBe(expected, undefined, new EmptyError());
    expectSubscriptions(source.subscriptions).toBe([sub1]);
  });
});
```

Entry 42, the no-argument test in the default-error describe block. Four silent frames, then the source completes and the output errors with EmptyError in that frame instead.

<!-- rxjs-visualizer:end -->
