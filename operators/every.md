# `every` Operator Behavior

## Operator Behavior in Plain Language

`every` tests whether all source values satisfy a predicate and emits only one final Boolean result.

Each source value is passed to the predicate. As soon as one value fails, `every` emits `false` and completes immediately because later values cannot change the answer.

If the source completes without a failure, `every` emits `true` and completes; an empty source therefore produces `true`. Source or predicate errors terminate the result.
