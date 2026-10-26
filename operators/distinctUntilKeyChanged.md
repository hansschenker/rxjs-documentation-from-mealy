# `distinctUntilKeyChanged` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`distinctUntilKeyChanged` suppresses consecutive source objects whose selected property has not changed.

The first source value is emitted and remembered. Each later value compares its configured property with that property on the previously emitted value, using strict equality by default or the supplied comparator.

If the keys compare equal, the value is suppressed. If they differ, the value is emitted and becomes the new previous value. Source completion and errors are forwarded.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `distinctUntilKeyChanged` run frame by frame](https://netxpert.ch/rxjs-visualizer/distinctUntilKeyChanged.html)**. The page runs the RxJS 7.8.2 marble test below, *should distinguish between values*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should distinguish between values', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const values = { a: { k: 1 }, b: { k: 2 }, c: { k: 3 } };
    const e1 = hot('  -a--b-b----a-c-|', values);
    const e1Subs = '  ^--------------!';
    const expected = '-a--b------a-c-|';

    const result = e1.pipe(distinctUntilKeyChanged('k'));

    expectObservable(result).toBe(expected, values);
    expectSubscriptions(e1.subscriptions).toBe(e1Subs);
  });
});
```

Only the key `k` is compared, and only with the value just before: the second b at 6 repeats the key 2 and is dropped; a at 11 is a different key from b and passes although an a was emitted earlier.

<!-- rxjs-visualizer:end -->
