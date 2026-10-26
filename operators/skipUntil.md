# `skipUntil` Operator Behavior

## Operator Behavior in Plain Language

`skipUntil` starts with its gate closed and subscribes to a notifier as well as the source.

Source values are suppressed while the gate is closed. The first notifier `next` opens the gate permanently, after which all later source values are forwarded.

If the notifier completes without emitting, the gate remains closed and source values continue to be skipped until the source itself completes. Source or notifier errors terminate the result.
