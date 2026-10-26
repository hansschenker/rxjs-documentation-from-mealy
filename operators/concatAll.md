# `concatAll` Operator Behavior

> New here? [How to read this page](../README.md#how-to-read-a-page) explains the two parts below. <!-- rxjs-visualizer:how-to-read -->

## Operator Behavior in Plain Language

`concatAll` receives inner Observables from an outer Observable and processes them one at a time.

When an inner arrives and none is active, it is subscribed immediately. Additional inners arriving while one is active are queued in arrival order.

Values from the active inner are forwarded. When it completes, the next queued inner starts. After the outer completes, active and queued inners are still allowed to finish; the result completes only when the outer is complete, no inner is active, and the queue is empty. Any outer or inner error terminates the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `concatAll` run frame by frame](https://netxpert.ch/rxjs-visualizer/concatAll.html)**. The page runs the RxJS 7.8.2 marble test below, *should concat an observable of observables*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should concat an observable of observables', () => {
  testScheduler.run(({ cold, hot, expectObservable }) => {
    const x = cold('    ----a------b------|                 ');
    const y = cold('                      ---c-d---|        ');
    const z = cold('                               ---e--f-|');
    const outer = hot('-x---y----z------|', { x: x, y: y, z: z });
    const expected = ' -----a------b---------c-d------e--f-|';

    const result = outer.pipe(concatAll());

    expectObservable(result).toBe(expected);
  });
});
```

Entry 45. y arrives at 5 and z at 10 while x is running: they wait in the queue (dotted) until 19 and 28, and their cold timelines start only then. The outer completes at 17; the queue drains after it.

<!-- rxjs-visualizer:end -->
