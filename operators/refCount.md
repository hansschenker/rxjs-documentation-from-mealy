# `refCount` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`refCount` manages the connection of a connectable Observable from the number of downstream subscribers.

When the first downstream subscriber arrives, `refCount` connects the connectable to its source. Additional subscribers share that same connection.

As subscribers leave, the reference count falls. When the final subscriber unsubscribes before the shared execution has already terminated, the source connection is disconnected. A later subscriber can therefore establish a new shared connection.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `refCount` run frame by frame](https://netxpert.ch/rxjs-visualizer/refCount.html)**. The page runs the RxJS 7.8.2 marble test below, *should turn a multicasted Observable an automatically (dis)connecting hot one*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should turn a multicasted Observable an automatically (dis)connecting hot one', () => {
  const testScheduler = new TestScheduler(observableMatcher);

  testScheduler.run(({ cold, expectObservable, expectSubscriptions }) => {
    const e1 = cold(' --1-2---3-4--5-|');
    const e1Subs = '  ^--------------!';
    const expected = '--1-2---3-4--5-|';

    const result = e1.pipe(publish(), refCount());

    expectObservable(result).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1Subs);
  });
});
```

The only marble test of refCount-spec.ts (the rest count `_refCount` with chai). `publish()` alone would leave the cold unsubscribed for ever: the bar on the source lane starts at 0 because refCount connected it for the first subscriber, and ends with the completion at 15. The counting itself is drawn by `refCountMany`, below.

A second page: **[refCountMany](https://netxpert.ch/rxjs-visualizer/refCountMany.html)**, *connects for the first subscriber, disconnects after the last, reconnects for the next (not an rxjs test)*.

<!-- rxjs-visualizer:end -->
