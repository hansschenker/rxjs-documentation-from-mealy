# `mergeMapTo` Operator Behavior

## Operator Behavior in Plain Language

`mergeMapTo` applies `mergeMap` concurrency behavior while mapping every source value to the same supplied inner Observable.

Each accepted source value causes a subscription to that inner. Several such subscriptions may overlap up to the concurrency limit; excess source values wait for capacity.

Values from all active subscriptions are forwarded as they arrive. Completion waits for the source, active subscriptions, and queued values. Errors terminate the result.
