# `concatMapTo` Operator Behavior

## Operator Behavior in Plain Language

`concatMapTo` has the same queueing policy as `concatMap`, but every source value maps to the same supplied inner Observable.

A source value starts a subscription to that inner when no inner is active. Further source values are queued while the current inner runs, and each queued value causes another subscription to the same inner after the previous subscription completes.

The result completes only after the source has completed and all queued inner subscriptions finish. Errors terminate the result.
