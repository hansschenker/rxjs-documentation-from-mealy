# `distinctUntilChanged` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`distinctUntilChanged` compares each source value only with the most recently emitted value.

The first value is always emitted and becomes the remembered previous value. For each later value, an optional key selector derives comparison keys and the comparator decides whether the previous emitted key and current key are equal.

Equal values are suppressed. A different value is emitted and becomes the new remembered value. Source completion and errors are forwarded; selector or comparator errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `distinctUntilChanged` run frame by frame](https://netxpert.ch/rxjs-visualizer/distinctUntilChanged.html)**. The page runs the RxJS 7.8.2 marble test below, *should distinguish between values*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should distinguish between values', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  -1--2-2----1-3-|');
    const e1subs = '  ^--------------!';
    const expected = '-1--2------1-3-|';

    expectObservable(e1.pipe(distinctUntilChanged())).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 31. Only the immediately repeated 2 at 6 is suppressed; the 1 at 11 passes again because the last emitted value was 2, not 1.

<!-- rxjs-visualizer:end -->
