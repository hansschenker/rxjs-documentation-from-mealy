# `raceWith` Operator Behavior

## Operator Behavior in Plain Language

`raceWith` subscribes to the source and all supplied competitors at the same time.

The first Observable to emit a `next`, `error`, or `complete` notification wins. The losing subscriptions are unsubscribed, and the result mirrors the winning Observable thereafter when it remains active.

Thus a first value selects a stream to follow, while an early error or completion can terminate the result immediately.
