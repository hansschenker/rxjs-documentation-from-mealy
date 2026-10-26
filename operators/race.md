# `race` Operator Behavior

## Operator Behavior in Plain Language

The pipeable `race` operator mirrors whichever participant wins the race among the source and supplied Observable inputs.

All participants are subscribed. The first participant to produce a `next`, `error`, or `complete` notification determines the outcome: competing subscriptions are abandoned, and the winner is mirrored from that point when applicable.

A winning value source continues to provide its later values and terminal notification. A winning error errors immediately, while a winning completion can complete the result before any value is emitted.
