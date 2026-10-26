# `combineAll` Operator Behavior

## Operator Behavior in Plain Language

`combineAll` is the deprecated name for the behavior now exposed by `combineLatestAll`.

It collects inner Observables from the outer Observable and waits for the outer to complete before combining them.

After every inner has emitted at least once, every new inner emission produces the latest value from every inner. The result completes when all participating inners complete, and an outer or inner error terminates the result.
