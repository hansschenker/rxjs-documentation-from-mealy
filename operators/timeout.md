# `timeout` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`timeout` watches whether source values arrive within configured deadlines.

A `first` deadline can constrain only the first source value. An `each` duration can constrain the gap between later values and, when no separate `first` setting is supplied, can also constrain the first value.

Each timely source value is forwarded and updates the timeout state. If a deadline expires first, the source subscription is cancelled. By default the result errors with `TimeoutError`; when a `with` factory is configured, `timeout` switches to that replacement Observable instead.

Normal source completion completes the result, and source or replacement errors are forwarded. The configured scheduler supplies the clock.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `timeout` run frame by frame](https://netxpert.ch/rxjs-visualizer/timeout.html)**. The page runs the RxJS 7.8.2 marble test below, *should timeout after a specified timeout period between emit with default error while source emits*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
const defaultTimeoutError = new TimeoutError();

it('should timeout after a specified timeout period between emit with default error while source emits', () => {
  rxTestScheduler.run(({ hot, expectObservable, expectSubscriptions, time }) => {
    const e1 = hot('  ---a---b---c------d---e---|');
    const t = time('             -----|');
    const e1subs = '  ^---------------!          ';
    const expected = '---a---b---c----#          ';

    const result = e1.pipe(timeout(t, rxTestScheduler));

    expectObservable(result).toBe(expected, undefined, defaultTimeoutError);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 13. A 5-frame deadline runs from subscription and restarts on every value; a, b and c each arrive in time and cancel theirs. Nothing follows c within 5 frames, so the deadline fires at 16: TimeoutError, the source dropped, d and e never seen.

<!-- rxjs-visualizer:end -->
