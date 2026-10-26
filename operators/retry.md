# `retry` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`retry` forwards values from the current source subscription, but treats a source error as a possible request to resubscribe.

If retry capacity remains, the failed subscription is torn down and a fresh subscription to the source is started immediately or after the configured delay. Values emitted before a failed attempt remain part of the output; a later attempt starts the source again from its own beginning.

When the retry count is exhausted, the error is forwarded. Normal source completion completes immediately. With a notifier delay, notifier emission retries, notifier completion completes the result, and notifier error errors the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `retry` run frame by frame](https://netxpert.ch/rxjs-visualizer/retry.html)**. The page runs the RxJS 7.8.2 marble test below, *should handle a basic source that emits next then errors, count=3*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should handle a basic source that emits next then errors, count=3', () => {
  rxTest.run(({ cold, expectObservable, expectSubscriptions }) => {
    const source = cold('--1-2-3-#');
    const subs = [
      '                  ^-------!                ',
      '                  --------^-------!        ',
      '                  ----------------^-------!',
    ];
    const expected = '   --1-2-3---1-2-3---1-2-3-#';

    const result = source.pipe(retry(2));

    expectObservable(result).toBe(expected);
    expectSubscriptions(source.subscriptions).toBe(subs);
  });
});
```

Entry 37. retry(2) is one subscription plus two retries, three attempts in all, one row each: the error at 8 resubscribes the cold at 8, the error at 16 again at 16, and the third error at 24 is passed on.

<!-- rxjs-visualizer:end -->
