# `distinct` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`distinct` remembers every comparison key that has already been accepted.

For each source value it derives a key, using the key selector when supplied. If that key has not been seen, the key is stored and the original value is emitted; if it has been seen, the value is suppressed.

When a flushes Observable is supplied, each flush emission clears the remembered key set so old keys may pass again. Source, selector, or flush errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `distinct` run frame by frame](https://netxpert.ch/rxjs-visualizer/distinct.html)**. The page runs the RxJS 7.8.2 marble test below, *should distinguish between values*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should distinguish between values', () => {
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  --a--a--a--b--b--a--|');
    const e1subs = '  ^-------------------!';
    const expected = '--a--------b--------|';

    expectObservable(e1.pipe(distinct())).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 49. Unlike distinctUntilChanged, the final a at 17 is suppressed too: every value seen before in this subscription stays remembered.

<!-- rxjs-visualizer:end -->
