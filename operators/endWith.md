# `endWith` Operator Behavior

## Operator Behavior in Plain Language

`endWith` forwards every source value unchanged while the source is active.

When the source completes normally, `endWith` emits the configured ending values in the order supplied and then completes.

If the source errors, the error is forwarded immediately and the ending values are not emitted.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `endWith` run frame by frame](https://netxpert.ch/rxjs-visualizer/endWith.html)**. The page runs the RxJS 7.8.2 marble test below, *should append to a cold Observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should append to a cold Observable', () => {
  testScheduler.run(({ cold, expectObservable, expectSubscriptions }) => {
    const e1 = cold(' ---a--b--c--|   ');
    const e1subs = '  ^-----------!   ';
    const expected = '---a--b--c--(s|)';

    expectObservable(e1.pipe(endWith('s'))).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Reserve list, endWith-spec.ts at tag 7.8.2, the first test. The appended value shares the completion's frame: s, then |, both at 12.

<!-- rxjs-visualizer:end -->
