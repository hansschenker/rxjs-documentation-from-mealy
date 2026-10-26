# `flatMap` Operator Behavior

## Operator Behavior in Plain Language

`flatMap` is the deprecated alias for `mergeMap` and has the same behavior.

Each source value is projected to an inner Observable. Multiple projected inners may run concurrently, subject to the concurrency limit, and their values are forwarded as they arrive.

If the concurrency limit is full, source values wait until capacity becomes available. Completion waits for the source, active inners, and queued work. Source, projection, or inner errors terminate the result.
