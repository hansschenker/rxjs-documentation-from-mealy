# `mergeWith` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`mergeWith` subscribes to the source and the supplied Observables concurrently.

A value from any participant is forwarded immediately, so values from different participants are interleaved according to delivery order.

The result completes only after every participant completes. An error from any participant terminates the result and tears down the other subscriptions.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `mergeWith` run frame by frame](https://netxpert.ch/rxjs-visualizer/mergeWith.html)**. The page runs the RxJS 7.8.2 marble test below, *should handle merging two hot observables*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should handle merging two hot observables', () => {
  rxTestScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --a-----b-----c----|');
    const e1subs = '  ^------------------!';
    const e2 = hot('-----d-----e-----f---|');
    const e2subs = '  ^--------------------!';
    const expected = '--a--d--b--e--c--f---|';

    const result = e1.pipe(mergeWith(e2));

    expectObservable(result).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
    expectSubscriptions(e2.subscriptions).toBe(e2subs);
  });
});
```

Entry 20. Both inputs are subscribed at once and every value is forwarded as it arrives; the result completes with the later input at 21. The e2 string sits two columns left in the upstream file, which changes nothing: run mode ignores the whitespace.

<!-- rxjs-visualizer:end -->
