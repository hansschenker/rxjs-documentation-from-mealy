# `windowTime` Operator Behavior

## Operator Behavior in Plain Language

`windowTime` opens and closes nested Observable windows according to time.

A window stays open for the configured span and receives every source value that arrives while it is active. With a creation interval, new windows can open periodically and overlap; with a maximum window size, a window can close early.

Each new window is emitted immediately from the outer Observable. Source completion completes all active windows and then the outer result. Timing comes from the configured scheduler, and source errors error the active windows and outer result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `windowTime` run frame by frame](https://netxpert.ch/rxjs-visualizer/windowTime.html)**. The page runs the RxJS 7.8.2 marble test below, *should emit windows given windowTimeSpan*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should emit windows given windowTimeSpan', () => {
  rxTestScheduler.run(({ hot, cold, time, expectSubscriptions, expectObservable }) => {
    const source = hot('--1--2--^--a--b--c--d--e--f--g--h--|');
    const subs = '              ^--------------------------!';
    const timeSpan = time('     ----------|                 ');
    //  10 frames               0---------1---------2------|
    //                                    ----------|
    //                                              ----------|
    const expected = '          x---------y---------z------|';
    const x = cold('            ---a--b--c|                 ');
    const y = cold('                      --d--e--f-|       ');
    const z = cold('                                -g--h--|');
    const values = { x, y, z };

    const result = source.pipe(windowTime(timeSpan, rxTestScheduler));

    expectObservable(result).toBe(expected, values);
    expectSubscriptions(source.subscriptions).toBe(subs);
  });
});
```

bufferTime's picture with windows instead of arrays: a window opens at the subscription and every 10 frames after it, on a clock of its own, and the values fall into whichever is open; the last window is cut by the source completion at 27. Each window is a row under the output lane, with the expected cold's marbles as rings.

<!-- rxjs-visualizer:end -->
