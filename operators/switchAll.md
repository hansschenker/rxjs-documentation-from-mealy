# `switchAll` Operator Behavior

## Operator Behavior in Plain Language

`switchAll` keeps at most one inner Observable active: the most recently emitted inner from the outer Observable.

The first inner is subscribed. Whenever a newer inner arrives, the current inner subscription is cancelled and the new inner becomes active.

Only values from the current inner are forwarded. If the outer completes while an inner remains active, that inner is allowed to continue; the result completes when both the outer has completed and no active inner remains. Outer or active-inner errors terminate the result.
