# `repeat` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`repeat` resubscribes to the source after normal completion.

Values from each source execution are forwarded as they occur. When one execution completes, `repeat` either completes because the configured repeat count has been exhausted, or waits for the configured delay/notifier and then starts a fresh source subscription.

Errors are not repeated: a source error is forwarded immediately. A delay notifier can trigger the next subscription, complete to stop repeating, or error to terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `repeat` run frame by frame](https://netxpert.ch/rxjs-visualizer/repeat.html)**. The page runs the RxJS 7.8.2 marble test below, *should resubscribe count number of times*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should resubscribe count number of times', () => {
  rxTest.run(({ cold, expectObservable, expectSubscriptions }) => {
    const e1 = cold(' --a--b--|                ');
    const subs = [
      '               ^-------!                ', //
      '               --------^-------!        ',
      '               ----------------^-------!',
    ];
    const expected = '--a--b----a--b----a--b--|';

    expectObservable(e1.pipe(repeat(3))).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(subs);
  });
});
```

Entry 38. repeat(3) is three subscriptions in all, unlike retry's count: each completion at 8 and 16 resubscribes the cold, only the third one completes the output.

<!-- rxjs-visualizer:end -->
