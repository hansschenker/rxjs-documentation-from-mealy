# `defaultIfEmpty` Operator Behavior

## Operator Behavior in Plain Language

`defaultIfEmpty` remembers whether the source has emitted any value.

Every source value is forwarded unchanged. If the source completes after at least one value, the result simply completes.

If the source completes without ever emitting, `defaultIfEmpty` emits the configured default value and then completes. A source error is forwarded and no default value is emitted.
