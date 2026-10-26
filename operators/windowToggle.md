# `windowToggle` Operator Behavior

## Operator Behavior in Plain Language

`windowToggle` can keep several nested Observable windows open at once.

Each emission from the openings Observable creates and emits a new window. The closing selector returns a closing Observable dedicated to that window.

Every source value is sent to every currently open window. When a window's closing Observable emits or completes, that window completes and is removed. Source completion closes all windows and completes the outer result; source, opening, selector, or closing errors terminate the affected result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `windowToggle` run frame by frame](https://netxpert.ch/rxjs-visualizer/windowToggle.html)**. The page runs the RxJS 7.8.2 marble test below, *should emit windows using varying hot closings*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should emit windows using varying hot closings', () => {
  rxTestScheduler.run(({ hot, cold, expectObservable, expectSubscriptions }) => {
    const e2 = cold('    --x-----------y--------z---|           ');
    const e2subs = '     ^--------------------------!           ';
    const closings = [
      hot('          -1--^----------------s-|                   '),
      hot('              -----3----4-------(s|)                 '),
      hot('              -------3----4-------5----------------s|'),
    ];
    const closingSubs = [
      '                  --^--------------!                     ',
      '                  --------------^---!                    ',
      '                  -----------------------^-----------!   ',
    ];

    const e1 = hot('--a--^---b---c---d---e---f---g---h------|   ');
    const e1subs = '     ^----------------------------------!   ';
    const expected = '   --x-----------y--------z-----------|   ';
    const x = cold('       --b---c---d---e|                     ');
    const y = cold('                   --e-|                    ');
    const z = cold('                            -g---h------|   ');
    const values = { x, y, z };

    let i = 0;
    const result = e1.pipe(windowToggle(e2, () => closings[i++]));

    expectObservable(result).toBe(expected, values);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
    expectSubscriptions(e2.subscriptions).toBe(e2subs);
    expectSubscriptions(closings[0].subscriptions).toBe(closingSubs[0]);
    expectSubscriptions(closings[1].subscriptions).toBe(closingSubs[1]);
    expectSubscriptions(closings[2].subscriptions).toBe(closingSubs[2]);
  });
});
```

Every value of the openings e2 opens a window and subscribes the next of the three closings; the closing's first value ends that window. Windows may overlap: the first runs 2 to 17 and the second 14 to 18, so e at 16 is in both. The 3, 4 and 5 of the closings fall outside their subscriptions and drop nothing; only the s values do.

<!-- rxjs-visualizer:end -->
