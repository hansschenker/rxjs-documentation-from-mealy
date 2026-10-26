# `skip` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`skip` suppresses the first configured number of source values.

It counts source emissions from zero. While the count is below the skip amount, values are dropped; after the threshold is reached, every later source value is forwarded unchanged.

Source completion and errors are forwarded. The skipped values are not buffered for later use.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `skip` run frame by frame](https://netxpert.ch/rxjs-visualizer/skip.html)**. The page runs the RxJS 7.8.2 marble test below, *should skip values before a total*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should skip values before a total', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const source = hot('--a--b--c--d--e--|');
    const subs = '      ^----------------!';
    const expected = '  -----------d--e--|';

    expectObservable(source.pipe(skip(3))).toBe(expected);
    expectSubscriptions(source.subscriptions).toBe(subs);
  });
});
```

Entry 27. The first three values are discarded, the rest pass at their own frames; skipping does not end the subscription early.

<!-- rxjs-visualizer:end -->
