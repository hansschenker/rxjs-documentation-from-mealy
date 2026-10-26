# `partition` Operator Behavior

## Operator Behavior in Plain Language

`partition` splits one logical source into two Observables according to a predicate.

The first returned Observable emits source values for which the predicate is true. The second emits values for which the predicate is false.

Each returned Observable applies its own filtering subscription to the source unless the source has already been explicitly shared. Source completion and errors flow to each subscribed branch, and predicate errors terminate the affected subscription.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `partition` run frame by frame](https://netxpert.ch/rxjs-visualizer/partition.html)**. The page runs the RxJS 7.8.2 marble test below, *an observable of integers into odd and even (not an rxjs test)*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should partition an observable of integers into even and odd', () => {
  rxTestScheduler.run(({ hot, expectSubscriptions }) => {
    const e1 = hot('   --1-2---3------4--5---6--|');
    const e1subs = '   ^------------------------!';
    // prettier-ignore
    const expected = [
      '                --1-----3---------5------|',
      '                ----2----------4------6--|',
    ];

    const result = partition(e1, (x: any) => x % 2 === 1);

    expectObservableArray(result, expected);
    expectSubscriptions(e1.subscriptions).toBe([e1subs, e1subs]);
  });
});
```

Not an rxjs test. partition-spec.ts (spec/observables) tests the creation function, `partition(e1, predicate)`, against two expected strings, and the deprecated operator of the same name, `partition(predicate)`, returns the same pair of observables; the operator has no marble test of its own. This spec applies the operator and emits the pair with `of(odd, even)`, so both halves are drawn the way groupBy's groups are, with the marbles of the creation function's first test:

Both halves subscribe the source separately, which is why its log has two subscriptions; each half is the source filtered, so every value lands in exactly one of the two rows.

<!-- rxjs-visualizer:end -->
