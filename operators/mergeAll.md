# `mergeAll` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`mergeAll` subscribes to inner Observables emitted by an outer Observable and allows several inners to be active at the same time.

Each accepted inner is subscribed up to the configured concurrency limit. If the limit is reached, additional inners wait in a queue until an active inner completes.

Values from all active inners are forwarded as they arrive. The result completes when the outer is complete, every active inner is complete, and the queue is empty. Outer or inner errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `mergeAll` run frame by frame](https://netxpert.ch/rxjs-visualizer/mergeAll.html)**. The page runs the RxJS 7.8.2 marble test below, *should merge a hot observable of cold observables*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should merge a hot observable of cold observables', () => {
  testScheduler.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const x = cold('    --a---b--c---d--|      ');
    const xsubs = '   --^---------------!      ';
    const y = cold('           ----e---f--g---|');
    const ysubs = '   ---------^--------------!';
    const e1 = hot('  --x------y-------|       ', { x: x, y: y });
    const e1subs = '  ^----------------!       ';
    const expected = '----a---b--c-e-d-f--g---|';

    expectObservable(e1.pipe(mergeAll())).toBe(expected);
    expectSubscriptions(x.subscriptions).toBe(xsubs);
    expectSubscriptions(y.subscriptions).toBe(ysubs);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Entry 43. The source's values are the colds themselves, so its lane shows their marble characters. x is subscribed at 2 and y at 9 while x still runs; their values interleave, and the output completes with y at 24, after the outer's own completion at 17.

<!-- rxjs-visualizer:end -->
