# `timeInterval` Operator Behavior

## Operator Behavior in Plain Language

`timeInterval` measures elapsed scheduler time between consecutive source emissions.

At subscription time it records the starting time. When the first source value arrives, it emits that value together with the time since subscription. For each later source value, it emits the value with the time since the previous source emission.

The scheduler supplies the clock. Source completion and errors are forwarded after the corresponding interval values already emitted.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `timeInterval` run frame by frame](https://netxpert.ch/rxjs-visualizer/timeInterval.html)**. The page runs the RxJS 7.8.2 marble test below, *should record the time interval between source elements*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should record the time interval between source elements', () => {
  rxTestScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('--a--^b-c-----d--e--|');
    const e1subs = '     ^--------------!';
    const expected = '   -w-x-----y--z--|';
    const expectedValue = { w: 1, x: 2, y: 6, z: 3 };

    const result = e1.pipe(
      timeInterval(rxTestScheduler),
      map((x) => x.interval)
    );

    expectObservable(result).toBe(expected, expectedValue);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Each value carries the frames since the one before it, or since the subscription for the first: b at 1 is 1, c at 3 is 2, d at 9 is 6, e at 12 is 3. The map keeps only that interval; the scheduler is the clock.

<!-- rxjs-visualizer:end -->
