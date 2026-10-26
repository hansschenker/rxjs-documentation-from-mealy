# `switchMapTo` Operator Behavior

## Operator Behavior in Plain Language

`switchMapTo` applies `switchMap`'s only-latest policy while mapping every source value to the same supplied inner Observable.

Each source value starts a new subscription to that inner and cancels the previous active inner subscription, if one exists. Only values from the newest subscription are forwarded.

If the source completes while the latest inner is active, the result waits for that inner to complete. Source or active-inner errors terminate the result.
