# `find` Operator Behavior

## Operator Behavior in Plain Language

`find` searches source values in order for the first value that satisfies a predicate.

Values that do not match are suppressed. When the first matching value arrives, `find` emits it and completes immediately.

If the source completes without a match, `find` emits `undefined` and completes. A source error or predicate error terminates the result.
