# `repeatWhen` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`repeatWhen` turns source completion into a signal that can decide whether another source subscription should start.

Source values are forwarded normally. When the source completes, a completion notification is sent to the notifier Observable created by the user-supplied notifier function.

Each notifier emission causes a fresh subscription to the source. If the notifier completes, the result completes; if it errors, the result errors. Source errors are not converted into repeat signals and are forwarded immediately.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `repeatWhen` run frame by frame](https://netxpert.ch/rxjs-visualizer/repeatWhen.html)**. The page runs the RxJS 7.8.2 marble test below, *should handle a source with eventual complete using a hot notifier*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should handle a source with eventual complete using a hot notifier', () => {
  testScheduler.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const source = cold('-1--2--|');
    //                                 -1--2--|
    //                                              -1--2--|
    const subs = [
      '                  ^------!                          ',
      '                  -------------^------!             ',
      '                  --------------------------^------!',
    ];
    const notifier = hot('-------------r------------r-|    ');
    const expected = '    -1--2---------1--2---------1--2--|';

    const result = source.pipe(repeatWhen((notifications: any) => notifier));

    expectObservable(result).toBe(expected);
    expectSubscriptions(source.subscriptions).toBe(subs);
  });
});
```

repeat's attempt rows, with the resubscription decided by a notifier: the cold completes at 7 and waits; each r of the notifier, at 13 and 26, subscribes it again. The notifier completes at 28 while the third run is going; that run finishes and the output completes with it at 33.

<!-- rxjs-visualizer:end -->
