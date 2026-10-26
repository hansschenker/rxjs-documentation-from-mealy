# `finalize` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`finalize` mirrors the source values, completion, and errors without changing them.

It registers a callback that runs when the resulting subscription terminates, whether termination comes from normal completion, an error, or explicit unsubscription.

The callback is teardown behavior: it does not emit a value and it does not turn cancellation into completion. It is used for cleanup and side effects that must run when the subscription lifecycle ends.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `finalize` run frame by frame](https://netxpert.ch/rxjs-visualizer/finalize.html)**. The page runs the RxJS 7.8.2 marble test below, *should handle unsubscription*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should handle unsubscription', () => {
  const testScheduler = new TestScheduler(observableMatcher);
  testScheduler.run(({ hot, expectObservable, expectSubscriptions }) => {
    let executed = false;
    const e1 = hot('  --a--b--c--|');
    const e1subs = '  ^-----!     ';
    const expected = '--a--b-';
    const unsub = '   ------!';

    const result = e1.pipe(finalize(() => (executed = true)));

    expectObservable(result, unsub).toBe(expected);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);

    // manually flush so `finalize()` has chance to execute before the test is over.
    testScheduler.flush();
    expect(executed).to.be.true;
  });
});
```

Entry 39. The unsub marble ends the output's subscription at 6, before c and without a completion; the source is unsubscribed in that frame and the finalizer runs. The spec asserts `executed` with chai after a flush; here the finalizer reports to the probe, `true` being the value the spec sets, and the row shows it running once, at 6.

<!-- rxjs-visualizer:end -->
