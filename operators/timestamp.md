# `timestamp` Operator Behavior

## Operator Behavior in Plain Language

`timestamp` attaches the scheduler's current time to every source value.

Each source `next` produces one object containing the original value and a timestamp obtained from the configured scheduler at delivery time.

The operator does not delay or suppress values. Source completion and errors are forwarded unchanged.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `timestamp` run frame by frame](https://netxpert.ch/rxjs-visualizer/timestamp.html)**. The page runs the RxJS 7.8.2 marble test below, *should record the time stamp per each source elements*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should record the time stamp per each source elements', () => {
  rxTestScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  -b-c-----d--e--|');
    const e1subs = '  ^--------------!';
    const expected = '-w-x-----y--z--|';
    const expectedValue = { w: 1, x: 3, y: 9, z: 12 };

    const result = e1.pipe(
      timestamp(rxTestScheduler),
      map((x) => x.timestamp)
    );

    expectObservable(result).toBe(expected, expectedValue);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Each value carries the scheduler's clock when it arrived, which inside run() is its frame: the output values are the frame numbers themselves.

<!-- rxjs-visualizer:end -->
