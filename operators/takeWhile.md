# `takeWhile` Operator Behavior

## Operator Behavior in Plain Language

`takeWhile` forwards source values while a predicate remains true.

Each source value is tested in order. When the predicate first returns false, the result completes immediately and no later source values are observed.

By default the failing value is suppressed. With the inclusive option enabled, that first failing value is emitted once before completion. Source or predicate errors terminate the result.
