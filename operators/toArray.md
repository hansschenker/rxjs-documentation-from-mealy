# `toArray` Operator Behavior

## Operator Behavior in Plain Language

`toArray` collects every source value into one array and emits nothing while the source is active.

Each incoming value is appended to the internal collection in source order.

When the source completes normally, the complete array is emitted once and the result completes. An empty source therefore emits an empty array. If the source errors, the error is forwarded and no accumulated array is emitted.
