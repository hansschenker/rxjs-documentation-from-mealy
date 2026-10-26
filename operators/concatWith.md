# `concatWith` Operator Behavior

## Operator Behavior in Plain Language

`concatWith` emits the source and then the supplied Observables sequentially.

Only one Observable is subscribed at a time. Its values are forwarded directly, and its completion causes the next Observable to be subscribed.

The result completes after the final Observable completes. If any current Observable errors, the result errors immediately and later Observables are not subscribed.
