# `merge` Operator Behavior

## Operator Behavior in Plain Language

The pipeable `merge` operator subscribes to the source and additional Observable inputs so their executions may overlap.

Whenever any active participant emits a value, that value is forwarded immediately. Values are interleaved according to the order in which the participating Observables deliver them.

The result completes only after every participant has completed. An error from any participant terminates the merged result and unsubscribes the others.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `mergeWith` run frame by frame](https://netxpert.ch/rxjs-visualizer/mergeWith.html)** (the deprecated operator `merge(...others)` is `mergeWith(...others)`, so its visualization is `mergeWith`'s). The page runs the RxJS 7.8.2 marble test below, *should handle merging two hot observables*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

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
