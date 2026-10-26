# `groupBy` Operator Behavior

## Operator Behavior in Plain Language

`groupBy` partitions source values into keyed `GroupedObservable` streams.

For each source value it computes a key. The first value for a new key creates a new group, emits that group from the outer result, and routes the value into it. Later values with the same key are routed to the existing group.

Optional element, duration, and connector functions can transform group values, close groups, or choose the Subject used by each group. Source completion completes all open groups and the outer result. Source or user-function errors error the affected grouping process.
