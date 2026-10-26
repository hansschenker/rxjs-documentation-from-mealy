# `throwIfEmpty` Operator Behavior

## Operator Behavior in Plain Language

`throwIfEmpty` forwards every source value unchanged while remembering whether at least one value has been seen.

If the source completes after emitting a value, the result completes normally.

If the source completes without any value, `throwIfEmpty` creates an error using the supplied factory, or an `EmptyError` by default, and terminates with that error. Source errors are forwarded directly.
