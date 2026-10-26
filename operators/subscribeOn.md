# `subscribeOn` Operator Behavior

## Operator Behavior in Plain Language

`subscribeOn` schedules the act of subscribing to the source on the supplied scheduler, optionally after a delay.

The downstream subscription is created immediately, but the source execution does not begin until the scheduled subscription work runs. If the downstream subscription is cancelled before then, the source need not be subscribed at all.

`subscribeOn` changes subscription timing; it does not by itself move every downstream notification onto that scheduler. `observeOn` is the operator for scheduling notification delivery.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `subscribeOn` run frame by frame](https://netxpert.ch/rxjs-visualizer/subscribeOn.html)**. The page runs the RxJS 7.8.2 marble test below, *should start subscribe after specified delay*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should start subscribe after specified delay', () => {
  testScheduler.run(({ hot, time, expectObservable, expectSubscriptions }) => {
    const e1 = hot('    --a--b--|');
    const expected = '  -----b--|';
    const delay = time('---|     ');
    const sub = '       ---^----!';

    const result = e1.pipe(subscribeOn(testScheduler, delay));

    expectObservable(result).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(sub);
  });
});
```

The subscription itself is scheduled: the hot source is subscribed only at 3, so a at 2 is missed and b at 5 is the first value seen. Nothing is delayed after that.

<!-- rxjs-visualizer:end -->
