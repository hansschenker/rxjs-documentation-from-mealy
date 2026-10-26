# `zipWith` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`zipWith` zips the source together with the supplied Observables position by position.

Each participant's values are queued. Whenever every participant has at least one queued value, `zipWith` removes one from each and emits them together as an array.

No participant can skip ahead: faster streams wait in their queues for the slower ones. The result completes when any completed participant has no queued value remaining, because no further full tuple can be produced. Any participant error terminates the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `zipWith` run frame by frame](https://netxpert.ch/rxjs-visualizer/zipWith.html)**. The page runs the RxJS 7.8.2 marble test below, *should end once one observable completes and its buffer is empty*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should end once one observable completes and its buffer is empty', () => {
  rxTestScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    const e1 = hot('  ---a--b--c--|               ');
    const e1subs = '  ^-----------!               ';
    const e2 = hot('  ------d----e----f--------|  ');
    const e2subs = '  ^-----------------!         ';
    const e3 = hot('  --------h----i----j---------'); // doesn't complete
    const e3subs = '  ^-----------------!         ';
    const expected = '--------x----y----(z|)      '; // e1 complete and buffer empty
    const values = {
      x: ['a', 'd', 'h'],
      y: ['b', 'e', 'i'],
      z: ['c', 'f', 'j'],
    };

    expectObservable(e1.pipe(zipWith(e2, e3))).toBe(expected, values);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
    expectSubscriptions(e2.subscriptions).toBe(e2subs);
    expectSubscriptions(e3.subscriptions).toBe(e3subs);
  });
});
```

Entry 19. A tuple needs one value from each input, so x waits for h at 8. e1 completes at 12 with c still queued; the third tuple goes out at 18 when j arrives, and with e1 done and drained the result completes there and drops e2 and e3, which never got to finish.

<!-- rxjs-visualizer:end -->
