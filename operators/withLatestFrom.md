# `withLatestFrom` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`withLatestFrom` treats the source as the trigger and the supplied Observables as remembered context.

It subscribes to the other Observables and stores their latest values. Nothing can be emitted until every other Observable has produced at least one value.

After readiness is reached, only a source emission triggers output: that source value is combined with the current latest values from the others. Emissions from the other Observables merely update remembered state. Source completion completes the result; an error from any participant terminates it.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `withLatestFrom` run frame by frame](https://netxpert.ch/rxjs-visualizer/withLatestFrom.html)**. The page runs the RxJS 7.8.2 marble test below, *should combine events from cold observables*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should combine events from cold observables', () => {
  rxTestScheduler.run(({ cold, expectObservable, expectSubscriptions }) => {
    const e2 = cold(' --1--2-3-4---|   ');
    const e2subs = '  ^------------!   ';
    const e1 = cold(' -a--b-----c-d-e-|');
    const e1subs = '  ^---------------!';
    const expected = '----B-----C-D-E-|';

    const result = e1.pipe(withLatestFrom(e2, (a: string, b: string) => String(a) + String(b)));

    expectObservable(result).toBe(expected, { B: 'b1', C: 'c4', D: 'd4', E: 'e4' });
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
    expectSubscriptions(e2.subscriptions).toBe(e2subs);
  });
});
```

Entry 22. The same inputs as combineLatestWith, a different trigger: only e1 values emit, each with the latest e2 value at that moment. a at 1 is dropped because e2 has nothing yet, and e2's own values emit nothing.

<!-- rxjs-visualizer:end -->
