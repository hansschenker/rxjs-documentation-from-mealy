# `connect` Operator Behavior

## Operator Behavior in Plain Language

`connect` creates a shared source connection for the dataflow returned by a selector.

For each downstream subscription, a connector Subject is created and an Observable view of it is passed to the selector. The downstream subscriber listens to the selector's result, while one subscription connects the connector to the source.

Source values are therefore multicast inside that selector-defined dataflow. The connection is scoped to that resulting subscription rather than becoming a permanent global share. Source or selector errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `connect` run frame by frame](https://netxpert.ch/rxjs-visualizer/connect.html)**. The page runs the RxJS 7.8.2 marble test below, *should connect a source through a selector function*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should connect a source through a selector function', () => {
  rxTest.run(({ cold, time, expectObservable }) => {
    const source = cold('---a----b-----c---|');
    const d = time('        ---|');
    const expected = '   ---a--a-b--b--c--c|';

    const result = source.pipe(connect((shared) => merge(shared.pipe(delay(d)), shared)));

    expectObservable(result).toBe(expected);
  });
});
```

The selector uses the shared source twice, once delayed by d and once as it is, and the cold is still subscribed only once: every value appears on the output at its own frame and again 3 frames later.

<!-- rxjs-visualizer:end -->
