# `mergeAll` Operator Behavior

## Operator Behavior in Plain Language

`mergeAll` subscribes to inner Observables emitted by an outer Observable and allows several inners to be active at the same time.

Each accepted inner is subscribed up to the configured concurrency limit. If the limit is reached, additional inners wait in a queue until an active inner completes.

Values from all active inners are forwarded as they arrive. The result completes when the outer is complete, every active inner is complete, and the queue is empty. Outer or inner errors terminate the result.
