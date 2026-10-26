# `observeOn` Operator Behavior

## Operator Behavior in Plain Language

`observeOn` changes where and when downstream notifications are delivered, not when the source itself is subscribed.

Each source `next`, `error`, and `complete` notification is scheduled on the supplied scheduler, optionally with a delay, before being delivered downstream.

The source can therefore execute in its original context while observers receive notifications through another scheduler. Use `subscribeOn` instead when the timing of the source subscription itself must be scheduled.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `observeOn` run frame by frame](https://netxpert.ch/rxjs-visualizer/observeOn.html)**. The page runs the RxJS 7.8.2 marble test below, *should observe after specified delay*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should observe after specified delay', () => {
  testScheduler.run(({ hot, time, expectObservable, expectSubscriptions }) => {
    const e1 = hot('    --a----b-|   ');
    const e1subs = '    ^--------!   ';
    const delay = time('  ---|       ');
    //                         ---|
    //                           ---|
    const expected = '  -----a----b-|';

    expectObservable(e1.pipe(observeOn(testScheduler, delay))).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Every notification, the completion included, is re-scheduled on the given scheduler with the delay: a, b and the completion each come out 3 frames later. The source itself is subscribed at 0 and unsubscribed at 9 as usual.

<!-- rxjs-visualizer:end -->
