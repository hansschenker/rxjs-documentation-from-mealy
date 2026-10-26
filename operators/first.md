# `first` Operator Behavior

## Operator Behavior in Plain Language

`first` looks for the first source value, or the first value satisfying an optional predicate.

As soon as a qualifying value arrives, it is emitted and the result completes immediately. Later source values are never needed.

If the source completes before a qualifying value is found, `first` emits the supplied default value when one was provided; otherwise it errors with `EmptyError`. Source or predicate errors terminate the result.
