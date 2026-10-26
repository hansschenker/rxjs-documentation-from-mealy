# `concatMap` Operator Behavior

## Operator Behavior in Plain Language

`concatMap` starts with no active inner stream and an empty queue.

When a source value arrives and no inner is active, the projection function creates an inner Observable and `concatMap` subscribes to it. Source values arriving while an inner is active wait in order rather than starting overlapping work.

Inner values are forwarded downstream. When an inner completes, the next queued source value is projected. If the source completes, queued work still finishes before the result completes. Source, projection, or inner errors terminate the result.
