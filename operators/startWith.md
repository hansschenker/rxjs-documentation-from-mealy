# `startWith` Operator Behavior

## Operator Behavior in Plain Language

`startWith` emits the configured values before it begins forwarding values from the source.

For each downstream subscription, the start values are delivered in the order supplied and only then is the source sequence observed.

After that prefix, source values and lifecycle notifications pass through normally. A source error still errors the result; the prefixed values are not a recovery mechanism.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `startWith` run frame by frame](https://netxpert.ch/rxjs-visualizer/startWith.html)**. The page runs the RxJS 7.8.2 marble test below, *should prepend to a cold Observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should prepend to a cold Observable', () => {
  testScheduler.run(({ cold, expectObservable, expectSubscriptions }) => {
    const e1 = cold(' ---a--b--c--|');
    const e1subs = '  ^-----------!';
    const expected = 's--a--b--c--|';

    const result = e1.pipe(startWith('s'));

    expectObservable(result).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Reserve list, startWith-spec.ts at tag 7.8.2, the first test. The prepended value is emitted synchronously at subscription, frame 0, before the cold source's first value at 3.

<!-- rxjs-visualizer:end -->
