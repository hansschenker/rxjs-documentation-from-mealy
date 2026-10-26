# `publish` Operator Behavior

## Operator Behavior in Plain Language

`publish` is a deprecated way to multicast a source through a plain `Subject`.

Without a selector, it returns a connectable Observable: subscribers can attach first, and an explicit `connect()` starts the single shared source subscription.

With a selector, the shared Subject-backed Observable is passed to the selector and the selector's dataflow is connected for the lifetime of that subscription. A plain Subject does not replay earlier values to late subscribers.
