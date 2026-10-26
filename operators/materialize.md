# `materialize` Operator Behavior

## Operator Behavior in Plain Language

`materialize` turns Observable notifications into ordinary `Notification` values.

Each source `next` becomes a `Notification` value describing that next emission. A source error is converted into an error `Notification` value, and source completion is converted into a completion `Notification` value.

After emitting the materialized error or completion notification, the resulting Observable completes normally. This makes lifecycle signals available as data.
