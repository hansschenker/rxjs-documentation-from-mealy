# `zip` Operator Behavior

## Operator Behavior in Plain Language

The pipeable `zip` operator combines the source with the supplied Observables position by position.

Each participant has a queue. A tuple is emitted only when every participant has at least one queued value; one value is then removed from each queue and combined in participant order.

Fast participants therefore wait for slower ones rather than overwriting their earlier values. The result completes when a participant has completed and its queue is empty, because no further complete tuple can then be formed. An error from any participant terminates the result.
