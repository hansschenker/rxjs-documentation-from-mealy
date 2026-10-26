# `windowCount` Operator Behavior

## Operator Behavior in Plain Language

`windowCount` opens windows according to source-value counts.

A window collects source values until it reaches the configured size, at which point that window completes. New windows start every configured start interval; therefore windows may be back-to-back, overlap, or leave gaps.

Each opened window is emitted immediately as a nested Observable. On source completion, all open windows complete and the outer result completes. Source errors error the open windows and outer result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `windowCount` run frame by frame](https://netxpert.ch/rxjs-visualizer/windowCount.html)**. The page runs the RxJS 7.8.2 marble test below, *should emit windows with count 3, no skip specified*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should emit windows with count 3, no skip specified', () => {
  rxTestScheduler.run(({ hot, cold, expectObservable, expectSubscriptions }) => {
    const source = hot('---a---b---c---d---e---f---g---h---i---|');
    const sourceSubs = '^--------------------------------------!';
    const expected = '  x----------y-----------z-----------w---|';
    const x = cold('    ---a---b---(c|)                         ');
    const y = cold('               ----d---e---(f|)             ');
    const z = cold('                           ----g---h---(i|) ');
    const w = cold('                                       ----|');
    const expectedValues = { x: x, y: y, z: z, w: w };

    const result = source.pipe(windowCount(3));

    expectObservable(result).toBe(expected, expectedValues);
    expectSubscriptions(source.subscriptions).toBe(sourceSubs);
  });
});
```

windowCount-spec.ts at tag 7.8.2, the first test. The output values are the windows themselves, like groupBy's groups: `expectedInners` carries the colds, and each window is a row under the output lane. The first window opens at subscription (x at 0) and each further one on the frame the third value closes the one before: y at 11 with c, z at 23 with f, w at 35 with i. w is the empty fourth window, closed by the completion.

<!-- rxjs-visualizer:end -->
