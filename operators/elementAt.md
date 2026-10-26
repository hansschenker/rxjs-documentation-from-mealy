# `elementAt` Operator Behavior

## Operator Behavior in Plain Language

`elementAt` counts source values from index zero and emits nothing before the requested position.

When the requested index arrives, that value is emitted and the result completes immediately because no later value can change the answer.

If the source completes before reaching the requested index, the supplied default value is emitted when one exists; otherwise the result errors with `ArgumentOutOfRangeError`. A source error is forwarded.
