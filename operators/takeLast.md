# `takeLast` Operator Behavior

## Operator Behavior in Plain Language

`takeLast` keeps only the final configured number of source values in a rolling buffer and emits nothing while the source is active.

As new values arrive, older values that can no longer belong to the final tail are discarded from the buffer.

When the source completes normally, the buffered tail is emitted in original source order and the result completes. If the source errors, the error is forwarded and the buffered values are not emitted.
