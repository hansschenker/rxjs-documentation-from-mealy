# `race` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

The pipeable `race` operator mirrors whichever participant wins the race among the source and supplied Observable inputs.

All participants are subscribed. The first participant to produce a `next`, `error`, or `complete` notification determines the outcome: competing subscriptions are abandoned, and the winner is mirrored from that point when applicable.

A winning value source continues to provide its later values and terminal notification. A winning error errors immediately, while a winning completion can complete the result before any value is emitted.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `raceWith` run frame by frame](https://netxpert.ch/rxjs-visualizer/raceWith.html)** (the deprecated operator `race(...others)` is `raceWith(...others)`, so its visualization is `raceWith`'s). The page runs the RxJS 7.8.2 marble test below, *should race cold and cold*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

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
