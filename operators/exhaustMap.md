# `exhaustMap` Operator Behavior

## Operator Behavior in Plain Language

`exhaustMap` starts with no active inner stream.

When a source value arrives while idle, the projection function creates an inner Observable and `exhaustMap` subscribes to it. While that inner remains active, later source values are ignored completely: they are neither queued nor used to cancel the current inner.

When the active inner completes, the next future source value may start new work. If the source completes while an inner is active, the result waits for it to finish. Source, projection, or active-inner errors terminate the result.
