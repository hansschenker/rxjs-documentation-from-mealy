# `concat` Operator Behavior

## Operator Behavior in Plain Language

The pipeable `concat` operator emits the source first and then the additional Observable inputs one at a time in the order supplied.

Only the current Observable is subscribed. Its values are forwarded immediately, and only when it completes does `concat` subscribe to the next Observable.

The result completes after the final Observable completes. If the current Observable errors, the result errors immediately and later Observables are never subscribed.
