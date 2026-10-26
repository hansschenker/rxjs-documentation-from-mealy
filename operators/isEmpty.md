# `isEmpty` Operator Behavior

## Operator Behavior in Plain Language

`isEmpty` determines whether the source emits any value.

If the first source value arrives, `isEmpty` emits `false` and completes immediately because the answer is known.

If the source completes before any value arrives, it emits `true` and completes. A source error is forwarded without producing a Boolean result.
