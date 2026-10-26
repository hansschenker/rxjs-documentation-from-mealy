# `catchError` Operator Behavior

## Operator Behavior in Plain Language

`catchError` forwards source values unchanged while the source runs normally.

If the source errors, it unsubscribes from that failed execution and calls the selector with the error and the caught Observable. The selector returns another Observable input, and `catchError` subscribes to it as the replacement.

Values, completion, and errors from the replacement are then forwarded downstream. If the selector throws, that error is forwarded. Normal source completion does not invoke the selector.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `catchError` run frame by frame](https://netxpert.ch/rxjs-visualizer/catchError.html)**. The page runs the RxJS 7.8.2 marble test below, *should catch error and replace it with a cold Observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should catch error and replace it with a cold Observable', () => {
  testScheduler.run(({ hot, cold, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --a--b--#          ');
    const e1subs = '  ^-------!          ';
    const e2 = cold('         1-2-3-4-5-|');
    const e2subs = '  --------^---------!';
    const expected = '--a--b--1-2-3-4-5-|';

    const result = e1.pipe(catchError((err: any) => e2));

    expectObservable(result).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
    expectSubscriptions(e2.subscriptions).toBe(e2subs);
  });
});
```

Entry 36. e1 errors at 8 (the red cross) and its subscription ends there; the cold replacement is subscribed in the same frame, so its 1 lands at 8, and the output completes with it at 18.

<!-- rxjs-visualizer:end -->
