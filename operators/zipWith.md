# `zipWith` Operator Behavior

## Operator Behavior in Plain Language

`zipWith` zips the source together with the supplied Observables position by position.

Each participant's values are queued. Whenever every participant has at least one queued value, `zipWith` removes one from each and emits them together as an array.

No participant can skip ahead: faster streams wait in their queues for the slower ones. The result completes when any completed participant has no queued value remaining, because no further full tuple can be produced. Any participant error terminates the result.
