# `tap` Operator Behavior

## Operator Behavior in Plain Language

`tap` mirrors the source while running user-supplied side-effect callbacks around the subscription lifecycle and notifications.

For each source value, the appropriate `next` callback runs and the original value is then forwarded unchanged. Error and completion callbacks observe the matching lifecycle signal without replacing it.

`tap` can also observe subscription, explicit unsubscription, and finalization through the supported observer callbacks. If a callback invoked by `tap` throws during notification handling, that error becomes an error of the resulting Observable.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `tap` run frame by frame](https://netxpert.ch/rxjs-visualizer/tap.html)**. The page runs the RxJS 7.8.2 marble test below, *should mirror multiple values and complete*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should mirror multiple values and complete', () => {
  testScheduler.run(({ cold, expectObservable, expectSubscriptions }) => {
    const e1 = cold(' --1--2--3--|');
    const e1subs = '  ^----------!';
    const expected = '--1--2--3--|';

    const result = e1.pipe(
      tap(() => {
        //noop
      })
    );

    expectObservable(result).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
  });
});
```

Not the document's entry 40 (an `of(1, 2, 3)` test with a done callback and no time axis; roadmap decision 3) but the suite's mirror test, the first of two with this title in tap-spec.ts on the 7.8.2 tag. The spec's callback is a noop; here it reports each call to the probe, so the row shows tap seeing 1, 2, 3 while the output mirrors the source unchanged.

<!-- rxjs-visualizer:end -->
