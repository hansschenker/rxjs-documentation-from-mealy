# `startWith` Operator Behavior

## Operator Behavior in Plain Language

`startWith` emits the configured values before it begins forwarding values from the source.

For each downstream subscription, the start values are delivered in the order supplied and only then is the source sequence observed.

After that prefix, source values and lifecycle notifications pass through normally. A source error still errors the result; the prefixed values are not a recovery mechanism.
