# `merge` Operator Behavior

## Operator Behavior in Plain Language

The pipeable `merge` operator subscribes to the source and additional Observable inputs so their executions may overlap.

Whenever any active participant emits a value, that value is forwarded immediately. Values are interleaved according to the order in which the participating Observables deliver them.

The result completes only after every participant has completed. An error from any participant terminates the merged result and unsubscribes the others.
