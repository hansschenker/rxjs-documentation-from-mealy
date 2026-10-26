# `switchMap` Operator Behavior

## Operator Behavior in Plain Language

`switchMap` starts with no active inner stream.

When an outer value arrives, that value is projected to an inner stream and `switchMap` subscribes to it.

While that inner is active, its values are forwarded downstream.

If another outer value arrives before the current inner completes, `switchMap` unsubscribes from the current inner and replaces it with the newly projected inner.

Only the currently active inner may contribute values.

If an inner completes while the outer is still running, `switchMap` returns to a state with no active inner and waits for another outer value.

If the outer completes while an inner is still active, `switchMap` allows that inner to continue. The result completes only after that final active inner completes.

An error from the outer stream or active inner stream terminates the operator with an error.
