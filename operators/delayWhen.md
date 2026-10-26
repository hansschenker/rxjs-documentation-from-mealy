# `delayWhen` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`delayWhen` chooses a separate delay for every source value.

For each value, the delay-duration selector returns an Observable associated with that value. The value is held until its duration Observable emits or completes, then it is released downstream.

Several values can be pending simultaneously, so later source values can be released before earlier ones when their durations finish sooner. The result completes after the source is complete and all pending values have been released. Source, selector, or duration errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `delayWhen` run frame by frame](https://netxpert.ch/rxjs-visualizer/delayWhen.html)**. The page runs the RxJS 7.8.2 marble test below, *should delay by selector*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should delay by selector', () => {
  testScheduler.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('     --a--b--| ');
    const expected = '   ---a--b-| ';
    const subs = '       ^-------! ';
    const selector = cold('-x--|   ');
    //                        -x--|
    // prettier-ignore
    const selectorSubs = [
      '                  --^!      ',
      '                  -----^!   ',
    ];

    const result = e1.pipe(delayWhen((x: any) => selector));

    expectObservable(result).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(subs);
    expectSubscriptions(selector.subscriptions).toBe(selectorSubs);
  });
});
```

The duration selector is subscribed per value and the value is emitted when it first emits: the selector's x comes one frame after its subscription, so a at 2 comes out at 3 and b at 5 at 6, and each selector subscription is dropped at once (its `|` is never reached).

<!-- rxjs-visualizer:end -->
