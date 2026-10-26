# `windowWhen` Operator Behavior

## Operator Behavior in Plain Language

`windowWhen` maintains one current nested Observable window at a time.

It opens and emits an initial window, then creates a closing Observable from the closing selector. Source values are forwarded into the current window.

When the closing Observable emits or completes, the current window completes, a new window is opened and emitted, and a new closing Observable is created. Source completion completes the current window and outer result. Source, selector, or closing-stream errors terminate the result.

## Visualization

<!-- rxjs-visualizer:start -->
**[▶ Watch `windowWhen` run frame by frame](https://netxpert.ch/rxjs-visualizer/windowWhen.html)**. The page runs the RxJS 7.8.2 marble test below, *should emit windows that close and reopen*, through the real `TestScheduler` and draws it one column per virtual frame: the inputs on top, the operator's row in the middle (its windows, the values it reacts to or drops, the inner subscriptions it makes) and the output lane at the bottom, where rings are the expected values and fills what actually arrived. Press Start or drag the scrubber; hover a marble for its frame and value. The text next to the drawing explains the operator step by step with a runnable sample.

```ts
it('should emit windows that close and reopen', () => {
  rxTestScheduler.run(({ hot, cold, expectObservable, expectSubscriptions }) => {
    const e2 = cold('       -----------|                ');
    //                                 -----------|
    //                                            -----------|
    const e2subs = [
      '                     ^----------!                ',
      '                     -----------^----------!     ',
      '                     ----------------------^----!',
    ];
    const e1 = hot('   --a--^--b--c--d--e--f--g--h--i--|');
    const e1subs = '        ^--------------------------!';
    const expected = '      a----------b----------c----|';

    const a = cold('        ---b--c--d-|                ');
    const b = cold('                   -e--f--g--h|     ');
    const c = cold('                              --i--|');
    const values = { a: a, b: b, c: c };

    const source = e1.pipe(windowWhen(() => e2));

    expectObservable(source).toBe(expected, values);
    expectSubscriptions(e1.subscriptions).toBe(e1subs);
    expectSubscriptions(e2.subscriptions).toBe(e2subs);
  });
});
```

bufferWhen's picture with windows instead of arrays: the closing is subscribed at 0 and again whenever it completes, at 11 and 22, and each completion ends one window and opens the next. The first window is emitted at the subscription, so the output has a value at 0; the last window is cut by the source completion at 27, as is the third closing.

<!-- rxjs-visualizer:end -->
