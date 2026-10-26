# `findIndex` Operator Behavior

## Operator Behavior in Plain Language

`findIndex` searches source values in order for the first value whose predicate returns true.

It tracks the zero-based source index while suppressing source values. On the first match, it emits that index and completes immediately.

If the source completes without a match, `findIndex` emits `-1` and completes. Source or predicate errors terminate the result.
