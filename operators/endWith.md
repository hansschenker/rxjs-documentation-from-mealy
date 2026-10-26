# `endWith` Operator Behavior

## Operator Behavior in Plain Language

`endWith` forwards every source value unchanged while the source is active.

When the source completes normally, `endWith` emits the configured ending values in the order supplied and then completes.

If the source errors, the error is forwarded immediately and the ending values are not emitted.
