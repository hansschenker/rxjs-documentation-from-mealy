# `dematerialize` Operator Behavior

## Operator Behavior in Plain Language

`dematerialize` treats source values as RxJS `Notification` objects and turns them back into real Observable notifications.

A `next` notification becomes a downstream value. An error notification terminates downstream with that error, and a completion notification completes downstream.

Once a terminal notification is dematerialized, no later source values can be observed downstream. A normal source error is also forwarded as an error.
