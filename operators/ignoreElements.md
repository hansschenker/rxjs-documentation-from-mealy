# `ignoreElements` Operator Behavior

## Operator Behavior in Plain Language

`ignoreElements` suppresses every source `next` value.

It does not suppress lifecycle signals: source completion is forwarded as completion and a source error is forwarded as an error.

The result therefore carries only the source's terminal behavior, never its ordinary values.
