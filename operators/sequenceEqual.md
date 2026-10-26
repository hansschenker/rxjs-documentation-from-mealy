# `sequenceEqual` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`sequenceEqual` compares two Observables as ordered sequences rather than comparing only their latest values.

It waits for values from both sides, compares corresponding positions with the supplied comparator or strict equality, and emits nothing while equality is still undecided.

The first unequal pair causes `false` to be emitted and the result completes. If one sequence ends with unmatched values, the result is also `false`. Only when both sequences complete with the same number of pairwise-equal values does it emit `true` and complete. Errors terminate the comparison.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `sequenceEqual` run frame by frame](https://netxpert.ch/rxjs-visualizer/sequenceEqual.html)**. The page runs the RxJS 7.8.2 marble test below, *should return true for two equal sequences*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
const booleans = { T: true, F: false };

it('should return true for two equal sequences', () => {
  rxTestScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const s1 = hot('--a--^--b--c--d--e--f--g--|       ');
    const s1subs = '     ^--------------------!       ';
    const s2 = hot('-----^-----b--c--d-e-f------g-|   ');
    const s2subs = '     ^------------------------!   ';
    const expected = '   -------------------------(T|)';

    const source = s1.pipe(sequenceEqual(s2));

    expectObservable(source).toBe(expected, booleans);
    expectSubscriptions(s1.subscriptions).toBe(s1subs);
    expectSubscriptions(s2.subscriptions).toBe(s2subs);
  });
});
```

The two streams carry the same values in the same order at different times; each value is matched against the other side's next unmatched one as it arrives. The answer, true, comes only when both have completed, at 25 with s2.

<!-- rxjs-visualizer:end -->
