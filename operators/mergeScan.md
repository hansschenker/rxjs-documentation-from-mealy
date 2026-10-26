# `mergeScan` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`mergeScan` combines accumulation with concurrent inner Observables.

For each accepted source value, the accumulator is called with the current accumulated state and returns an inner Observable. Inner subscriptions may overlap up to the configured concurrency limit.

Every value emitted by an active inner becomes the new accumulated state and is also emitted downstream. Because inners may overlap, state can be updated by whichever active inner emits next. Source completion waits for active and queued work; source, accumulator, or inner errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `mergeScan` run frame by frame](https://netxpert.ch/rxjs-visualizer/mergeScan.html)**. The page runs the RxJS 7.8.2 marble test below, *should mergeScan values and be able to asynchronously project them*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should mergeScan values and be able to asynchronously project them', () => {
  testScheduler.run(({ hot, time, expectObservable, expectSubscriptions }) => {
    const e1 = hot('--a--^--b--c--d--e--f--g--|');
    const e1subs = '     ^--------------------!';
    const t = time('        --|                '); // t = 2
    //                         --|
    //                            --|
    //                               --|
    //                                  --|
    //                                     --|
    const expected = '   -----u--v--w--x--y--z|';

    const values = {
      u: ['b'],
      v: ['b', 'c'],
      w: ['b', 'c', 'd'],
      x: ['b', 'c', 'd', 'e'],
      y: ['b', 'c', 'd', 'e', 'f'],
      z: ['b', 'c', 'd', 'e', 'f', 'g'],
    };

    const result = e1.pipe(mergeScan((acc, x) => of(acc.concat(x)).pipe(delay(t)), [] as string[]));

    expectObservable(result).toBe(expected, values);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Like scan, but each step is an observable: the accumulator returns of(acc.concat(x)) delayed by t, so every array comes out 2 frames after its value, and the next step starts from the last array emitted. The last step's delay runs past the source completion at 21: z and the completion share that frame.

<!-- rxjs-visualizer:end -->
