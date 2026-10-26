# `mergeMap` Operator Behavior

## Operator Behavior in Plain Language

`mergeMap` projects each source value to an inner Observable and allows projected inners to overlap.

When concurrency capacity is available, a source value is projected and its inner is subscribed immediately. If the configured concurrency limit is reached, later source values wait until an active inner completes.

Values from all active inners are forwarded as they arrive. The result completes only after the source is complete, all active inners are complete, and the queue is empty. Source, projection, or inner errors terminate the result.
