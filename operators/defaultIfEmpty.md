# `defaultIfEmpty` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`defaultIfEmpty` remembers whether the source has emitted any value.

Every source value is forwarded unchanged. If the source completes after at least one value, the result simply completes.

If the source completes without ever emitting, `defaultIfEmpty` emits the configured default value and then completes. A source error is forwarded and no default value is emitted.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `defaultIfEmpty` run frame by frame](https://netxpert.ch/rxjs-visualizer/defaultIfEmpty.html)**. The page runs the RxJS 7.8.2 marble test below, *should return the Observable if not empty with a default value*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should return the Observable if not empty with a default value', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --------|');
    const e1subs = '  ^-------!';
    const expected = '--------(x|)';

    expectObservable(e1.pipe(defaultIfEmpty(42))).toBe(expected, { x: 42 });
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Not the document's entry 41 (`cold(' |')`, a single frame with nothing for the playhead to walk; roadmap decision 6) but the first test of defaultIfEmpty-spec.ts on the 7.8.2 tag. Its title is misleading: the source is empty and completes after eight silent frames, and the default 42 leaves with the completion.

<!-- rxjs-visualizer:end -->
