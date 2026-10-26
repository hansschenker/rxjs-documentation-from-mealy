# `skipLast` Operator Behavior

## Operator Behavior in Plain Language

`skipLast` keeps a rolling buffer large enough to withhold the configured number of most recent source values.

Before the buffer exceeds that size, nothing is emitted. Once it is full, each new source value causes the oldest buffered value to be emitted while the new value becomes part of the withheld tail.

When the source completes, the final configured number of buffered values are discarded and the result completes. A source error terminates immediately without flushing that tail.
