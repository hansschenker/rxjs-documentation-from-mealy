# `mergeWith` Operator Behavior

## Operator Behavior in Plain Language

`mergeWith` subscribes to the source and the supplied Observables concurrently.

A value from any participant is forwarded immediately, so values from different participants are interleaved according to delivery order.

The result completes only after every participant completes. An error from any participant terminates the result and tears down the other subscriptions.
