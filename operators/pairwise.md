# `pairwise` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`pairwise` remembers the immediately previous source value.

The first source value is stored but nothing is emitted because there is no pair yet. For every later source value, `pairwise` emits `[previous, current]` and then makes the current value the new previous value.

Source completion and errors are forwarded. An empty source or a source with only one value produces no pair.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `pairwise` run frame by frame](https://netxpert.ch/rxjs-visualizer/pairwise.html)**. The page runs the RxJS 7.8.2 marble test below, *should group consecutive emissions as arrays of two*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should group consecutive emissions as arrays of two', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --a--b-c----d--e---|');
    const e1subs = '  ^------------------!';
    const expected = '-----u-v----w--x---|';

    const values = {
      u: ['a', 'b'],
      v: ['b', 'c'],
      w: ['c', 'd'],
      x: ['d', 'e'],
    };

    expectObservable(e1.pipe(pairwise())).toBe(expected, values);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 33. From the second value on, every value emits [previous, current] in its own frame; a alone emits nothing, and the pairs overlap by one.

<!-- rxjs-visualizer:end -->
