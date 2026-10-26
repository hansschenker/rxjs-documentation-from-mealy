# `skipWhile` Operator Behavior

## Operator Behavior in Plain Language

`skipWhile` starts in a skipping state.

Each source value is tested by the predicate while skipping remains active. Values for which the predicate returns true are suppressed.

The first value for which the predicate returns false ends the skipping state permanently; that value and every later source value are forwarded without further predicate checks. Source completion and errors are forwarded, while predicate errors terminate the result.
