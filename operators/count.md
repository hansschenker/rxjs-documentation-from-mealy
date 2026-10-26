# `count` Operator Behavior

## Operator Behavior in Plain Language

`count` starts at zero and emits nothing while the source is running.

For each source value, it increments the count directly or, when a predicate is supplied, only when the predicate returns true.

When the source completes, `count` emits the final count exactly once and then completes. A source error or predicate error terminates the result without emitting a final count.
