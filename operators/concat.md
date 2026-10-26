# `concat` Operator Behavior

## Operator Behavior in Plain Language

The pipeable `concat` operator emits the source first and then the additional Observable inputs one at a time in the order supplied.

Only the current Observable is subscribed. Its values are forwarded immediately, and only when it completes does `concat` subscribe to the next Observable.

The result completes after the final Observable completes. If the current Observable errors, the result errors immediately and later Observables are never subscribed.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `concatWith` run frame by frame](https://netxpert.ch/rxjs-visualizer/concatWith.html)** (the deprecated operator `concat(...others)` is `concatWith(...others)`, so its visualization is `concatWith`'s). The page runs the RxJS 7.8.2 marble test below, *should concatenate two cold observables*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should concatenate two cold observables', () => {
  rxTest.run(({ cold, expectObservable }) => {
    const e1 = cold(' --a--b-|');
    const e2 = cold('        --x---y--|');
    const expected = '--a--b---x---y--|';

    expectObservable(e1.pipe(concatWith(e2))).toBe(expected);
  });
});
```

Entry 21. e2 is cold, so its sequence starts when concatWith subscribes it, at 7 when e1 completes: the lane's marbles are shifted there, and x lands at 9, not at 2.

<!-- rxjs-visualizer:end -->
