# `materialize` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`materialize` turns Observable notifications into ordinary `Notification` values.

Each source `next` becomes a `Notification` value describing that next emission. A source error is converted into an error `Notification` value, and source completion is converted into a completion `Notification` value.

After emitting the materialized error or completion notification, the resulting Observable completes normally. This makes lifecycle signals available as data.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `materialize` run frame by frame](https://netxpert.ch/rxjs-visualizer/materialize.html)**. The page runs the RxJS 7.8.2 marble test below, *should materialize an Observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should materialize an Observable', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --x--y--z--|   ');
    const e1subs = '  ^----------!   ';
    const expected = '--a--b--c--(d|)';
    const values = { a: '{x}', b: '{y}', c: '{z}', d: '|' };

    const result = e1.pipe(
      materialize(),
      map((x: Notification<string>) => {
        if (x.kind === 'C') {
          return '|';
        } else {
          return '{' + x.value + '}';
        }
      })
    );

    expectObservable(result).toBe(expected, values);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Every event becomes a value: x, y and z arrive as next notifications, drawn `{x}`, and the source's completion at 11 arrives as a value too, `|`, after which the output completes in the same frame.

<!-- rxjs-visualizer:end -->
