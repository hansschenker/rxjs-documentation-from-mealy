# `sample` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`sample` remembers the latest source value that has arrived since the previous sample.

Source emissions update that remembered value but do not themselves trigger output. Each emission from the notifier asks for a sample: if a new source value has arrived since the previous sample, the latest one is emitted and the pending-sample flag is cleared.

Notifier emissions with no new source value produce nothing. The result completes when the source completes; source or notifier errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `sample` run frame by frame](https://netxpert.ch/rxjs-visualizer/sample.html)**. The page runs the RxJS 7.8.2 marble test below, *should get samples when the notifier emits*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should get samples when the notifier emits', () => {
  rxTest.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  ---a----b---c----------d-----|   ');
    const e1subs = '  ^----------------------------!   ';
    const e2 = hot('  -----x----------x---x------x---| ');
    const e2subs = '  ^----------------------------!   ';
    const expected = '-----a----------c----------d-|   ';

    expectObservable(e1.pipe(sample(e2))).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
    expectSubscriptions(e2.subscriptions).toBe(e2subs);
  });
});
```

Reserve list, sample-spec.ts at tag 7.8.2, the first test. Every notifier value drops onto the operator row; the ones at 5, 16 and 27 find a fresh source value and emit it, the one at 20 finds nothing new since 16 and emits nothing. Both inputs end when the source completes.

<!-- rxjs-visualizer:end -->
