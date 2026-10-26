# `timeoutWith` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`timeoutWith` is the deprecated convenience form of timeout behavior that switches to a supplied Observable when the timeout expires.

Source values are forwarded while they arrive within the configured timeout period. When the required value does not arrive in time, the source subscription is cancelled and `timeoutWith` subscribes to the fallback Observable.

From then on, the fallback's values and lifecycle notifications are forwarded. Source completion before timeout completes normally; source or fallback errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `timeoutWith` run frame by frame](https://netxpert.ch/rxjs-visualizer/timeoutWith.html)**. The page runs the RxJS 7.8.2 marble test below, *should timeout after a specified period then subscribe to the passed observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should timeout after a specified period then subscribe to the passed observable', () => {
  rxTestScheduler.run(({ cold, time, expectObservable, expectSubscriptions }) => {
    const source = cold('  -------a--b--|');
    const sourceSubs = '   ^----!        ';
    const t = time('       -----|');
    const switchTo = cold('     x-y-z-|  ');
    const switchToSubs = ' -----^-----!  ';
    const expected = '     -----x-y-z-|  ';

    const result = source.pipe(timeoutWith(t, switchTo, rxTestScheduler));

    expectObservable(result).toBe(expected);
    expectSubscriptions(source.subscriptions).toBe(sourceSubs);
    expectSubscriptions(switchTo.subscriptions).toBe(switchToSubs);
  });
});
```

timeout's deadline with a replacement instead of an error: the source has nothing in its first 5 frames, so at 5 it is unsubscribed (its a at 7 is never seen) and switchTo is subscribed, its marbles shifted to 5.

<!-- rxjs-visualizer:end -->
