# `debounceTime` Operator Behavior

## Operator Behavior in Plain Language

`debounceTime` keeps the latest source value pending until the source has been silent for the configured duration.

Each new source value replaces the pending value and moves the silence deadline forward. Only when no newer value arrives for the full duration is the latest pending value emitted.

If the source completes while a value is pending, that value is emitted before completion. Source errors terminate immediately. The configured scheduler supplies the clock and schedules the waiting period.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `debounceTime` run frame by frame](https://netxpert.ch/rxjs-visualizer/debounceTime.html)**. The page runs the RxJS 7.8.2 marble test below, *should debounce values by 2 time units*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should debounce values by 2 time units', () => {
  testScheduler.run(({ hot, time, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  -a--bc--d---|');
    const e1subs = '  ^-----------!';
    const expected = '---a---c--d-|';
    const t = time('  --|');
    expectObservable(e1.pipe(debounceTime(t))).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

<!-- rxjs-visualizer:end -->
