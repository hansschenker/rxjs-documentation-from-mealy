# `observeOn` Operator Behavior

## Operator Behavior in Plain Language

`observeOn` changes where and when downstream notifications are delivered, not when the source itself is subscribed.

Each source `next`, `error`, and `complete` notification is scheduled on the supplied scheduler, optionally with a delay, before being delivered downstream.

The source can therefore execute in its original context while observers receive notifications through another scheduler. Use `subscribeOn` instead when the timing of the source subscription itself must be scheduled.
