# `debounce` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`debounce` keeps at most one pending source value.

Each source value replaces the pending value and creates a new duration Observable. If another source value arrives before that duration signals, the previous duration subscription is cancelled and a new duration is created for the new value.

When the current duration emits or completes, the pending value is emitted and cleared. If the source completes while a value is pending, that value is emitted immediately before completion. Source, selector, or duration errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `debounce` run frame by frame](https://netxpert.ch/rxjs-visualizer/debounce.html)**. The page runs the RxJS 7.8.2 marble test below, *should debounce values by a specified cold Observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should debounce values by a specified cold Observable', () => {
  testScheduler.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  -a----bc----d-ef----|');
    const e1subs = '  ^-------------------!';
    const e2 = cold('  ---x                ');
    //                       ---x
    //                               ---x
    const e2subs = [
      '               -^--!                ',
      '               ------^!             ',
      '               -------^--!          ',
      '               ------------^-!      ',
      '               --------------^!     ',
      '               ---------------^--!  ',
    ];
    const expected = '----a-----c-------f-|';

    const result = e1.pipe(debounce(() => e2));

    expectObservable(result).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
    expectSubscriptions(e2.subscriptions).toBe(e2subs);
  });
});
```

Reserve list, debounce-spec.ts at tag 7.8.2, the first test. Every value subscribes the duration cold; a newer value unsubscribes the running one before its x, so b, d and e get a row cut short and only a, c and f see their x at 3 and are emitted then.

<!-- rxjs-visualizer:end -->
