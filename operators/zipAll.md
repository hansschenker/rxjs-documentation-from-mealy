# `zipAll` Operator Behavior

## Operator Behavior in Plain Language

`zipAll` receives inner Observables from an outer Observable and waits for the outer to complete before zipping those inners together.

After subscription to the collected inners, each inner has its own queue. A combined value is emitted only when every inner has an available value, consuming one value from each queue.

The result completes when no further full tuple can be formed, such as when a completed inner has no queued value left. Outer or inner errors terminate the result.
