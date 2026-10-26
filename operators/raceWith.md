# `raceWith` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`raceWith` subscribes to the source and all supplied competitors at the same time.

The first Observable to emit a `next`, `error`, or `complete` notification wins. The losing subscriptions are unsubscribed, and the result mirrors the winning Observable thereafter when it remains active.

Thus a first value selects a stream to follow, while an early error or completion can terminate the result immediately.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `raceWith` run frame by frame](https://netxpert.ch/rxjs-visualizer/raceWith.html)**. The page runs the RxJS 7.8.2 marble test below, *should race cold and cold*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should race cold and cold', () => {
  testScheduler.run(({ cold, expectObservable, expectSubscriptions }) => {
    const e1 = cold(' ---a-----b-----c----|   ');
    const e1subs = '  ^-------------------!   ';
    const e2 = cold(' ------x-----y-----z----|');
    const e2subs = '  ^--!                    ';
    const expected = '---a-----b-----c----|   ';

    const result = e1.pipe(raceWith(e2));

    expectObservable(result).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
    expectSubscriptions(e2.subscriptions).toBe(e2subs);
  });
});
```

Entry 23. Both are subscribed at 0; a at 3 settles the race for e1 and e2 is dropped in that frame, so x, y and z never happen: they stay on the lane past its bar and drop nothing onto the operator row.

<!-- rxjs-visualizer:end -->
