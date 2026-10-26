# `mergeMapTo` Operator Behavior

## Operator Behavior in Plain Language

`mergeMapTo` applies `mergeMap` concurrency behavior while mapping every source value to the same supplied inner Observable.

Each accepted source value causes a subscription to that inner. Several such subscriptions may overlap up to the concurrency limit; excess source values wait for capacity.

Values from all active subscriptions are forwarded as they arrive. Completion waits for the source, active subscriptions, and queued values. Errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `mergeMapTo` run frame by frame](https://netxpert.ch/rxjs-visualizer/mergeMapTo.html)**. The page runs the RxJS 7.8.2 marble test below, *should map-and-flatten each item to an Observable*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should map-and-flatten each item to an Observable', () => {
  testScheduler.run(({ cold, hot, expectObservable, expectSubscriptions }) => {
    const x = cold('    x-x-x|            ');
    //                        x-x-x|
    //                           x-x-x|
    // prettier-ignore
    const xsubs = [
      '               --^----!            ',
      '               --------^----!      ',
      '               -----------^----!   ',
    ];
    const e1 = hot('  --1-----3--5-------|');
    const e1subs = '  ^------------------!';
    const expected = '--x-x-x-x-xxxx-x---|';

    const result = e1.pipe(mergeMapTo(x));

    expectObservable(result).toBe(expected);
    expectSubscriptions(x.subscriptions).toBe(xsubs);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

mergeMap with the same inner for every value: the cold is subscribed at each outer value and the three runs overlap, the second and third sharing frames 11 and 13 (the `xx` pairs).

<!-- rxjs-visualizer:end -->
