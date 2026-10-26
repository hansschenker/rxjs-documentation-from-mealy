# `filter` Operator Behavior

## Operator Behavior in Plain Language

`filter` examines each source value independently with a predicate.

If the predicate returns true, the original value is forwarded unchanged. If it returns false, the value is suppressed. The predicate receives the source value and its zero-based source index.

`filter` keeps no business state beyond that index. Source completion and errors are forwarded; an error thrown by the predicate terminates the result.
