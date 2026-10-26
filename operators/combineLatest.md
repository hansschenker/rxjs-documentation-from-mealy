# `combineLatest` Operator Behavior

## Operator Behavior in Plain Language

The pipeable `combineLatest` operator combines the source with the additional Observable inputs supplied to it.

It subscribes to all participants and remembers the latest value from each. Nothing is emitted until every participant has emitted at least once. After that, every new value from any participant emits a combined array or selector result built from all current latest values.

A completed participant keeps contributing its last value if it produced one. The result completes when all participants complete. Any participant error terminates the result.
