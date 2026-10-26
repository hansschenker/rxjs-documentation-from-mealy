# `buffer` Operator Behavior

## Operator Behavior in Plain Language

`buffer` starts with an empty buffer and listens to both the source and a closing notifier.

Each source value is added to the current buffer instead of being emitted immediately. Whenever the closing notifier emits, `buffer` emits the current buffer as an array and immediately starts a new empty buffer.

When the source completes, the current buffer is emitted and the result completes. A source or notifier error terminates the result; completion of the notifier alone does not complete the source-driven result.
