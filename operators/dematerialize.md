# `dematerialize` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`dematerialize` treats source values as RxJS `Notification` objects and turns them back into real Observable notifications.

A `next` notification becomes a downstream value. An error notification terminates downstream with that error, and a completion notification completes downstream.

Once a terminal notification is dematerialized, no later source values can be observed downstream. A normal source error is also forwarded as an error.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `dematerialize` run frame by frame](https://netxpert.ch/rxjs-visualizer/dematerialize.html)**. The page runs the RxJS 7.8.2 marble test below, *should dematerialize an Observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should dematerialize an Observable', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const values = {
      a: '{x}',
      b: '{y}',
      c: '{z}',
      d: '|',
    };

    const e1 = hot('  --a--b--c--d-|', values);
    const e1subs = '  ^----------!  ';
    const expected = '--x--y--z--|  ';

    const result = e1.pipe(
      map((x: string) => {
        if (x === '|') {
          return Notification.createComplete();
        } else {
          return Notification.createNext(x.replace('{', '').replace('}', ''));
        }
      }),
      dematerialize()
    );

    expectObservable(result).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

The source carries notifications written as strings, `{x}` a next of x and `|` a completion; the map turns them into Notification objects and dematerialize into the events they describe. The output completes at 11, where the source emitted d, and the source's own completion at 13 is never seen: the subscription ends at 11.

<!-- rxjs-visualizer:end -->
