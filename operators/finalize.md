# `finalize` Operator Behavior

## Operator Behavior in Plain Language

`finalize` mirrors the source values, completion, and errors without changing them.

It registers a callback that runs when the resulting subscription terminates, whether termination comes from normal completion, an error, or explicit unsubscription.

The callback is teardown behavior: it does not emit a value and it does not turn cancellation into completion. It is used for cleanup and side effects that must run when the subscription lifecycle ends.
