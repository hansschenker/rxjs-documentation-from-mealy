# `skipWhile` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`skipWhile` starts in a skipping state.

Each source value is tested by the predicate while skipping remains active. Values for which the predicate returns true are suppressed.

The first value for which the predicate returns false ends the skipping state permanently; that value and every later source value are forwarded without further predicate checks. Source completion and errors are forwarded, while predicate errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `skipWhile` run frame by frame](https://netxpert.ch/rxjs-visualizer/skipWhile.html)**. The page runs the RxJS 7.8.2 marble test below, *should invoke predicate while its false and never again*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should invoke predicate while its false and never again', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const source = hot('--a--b-^-c--d--e--f--g--h--|');
    const sourceSubs = '       ^-------------------!';
    const expected = '         --------e--f--g--h--|';

    let invoked = 0;
    const predicate = function (v: string) {
      invoked++;
      return v !== 'e';
    };

    const result = source.pipe(
      skipWhile(predicate),
      tap({
        complete() {
          expect(invoked).to.equal(3);
        },
      })
    );

    expectObservable(result).toBe(expected);
    expectSubscriptions(source.subscriptions).toBe(sourceSubs);
  });
});
```

Entry 28. a and b happen before the `^` and are never seen. The tap with chai's expect is the test's assertion hook; here the predicate reports each call to the probe instead, and the operator row shows the three calls, on c, d and e: false on e opens the gate, f, g, h pass unasked.

<!-- rxjs-visualizer:end -->
