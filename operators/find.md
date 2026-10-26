# `find` Operator Behavior

## Operator Behavior in Plain Language

`find` searches source values in order for the first value that satisfies a predicate.

Values that do not match are suppressed. When the first matching value arrives, `find` emits it and completes immediately.

If the source completes without a match, `find` emits `undefined` and completes. A source error or predicate error terminates the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `find` run frame by frame](https://netxpert.ch/rxjs-visualizer/find.html)**. The page runs the RxJS 7.8.2 marble test below, *should return matching element from source emits multiple elements*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should return matching element from source emits multiple elements', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --a--b---c-|');
    const e1subs = '  ^----!      ';
    const expected = '-----(b|)   ';

    const predicate = function (value: string) {
      return value === 'b';
    };

    expectObservable(e1.pipe(find(predicate))).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Reserve list, find-spec.ts at tag 7.8.2. The predicate reports each call to the probe: it runs on a and b, b matches, and c is never asked.

<!-- rxjs-visualizer:end -->
