# `takeUntil` Operator Behavior

## Operator Behavior in Plain Language

`takeUntil` forwards source values while waiting for a notifier to emit.

If the notifier emits a value, `takeUntil` completes the result immediately and unsubscribes from the source; the notifier value itself is not emitted.

If the notifier merely completes without emitting, it does not stop the source and source values continue normally. Source completion completes the result, while an error from either source or notifier terminates it with an error.
