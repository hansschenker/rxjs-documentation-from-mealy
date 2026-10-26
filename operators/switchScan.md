# `switchScan` Operator Behavior

## Operator Behavior in Plain Language

`switchScan` combines accumulated state with `switchMap` cancellation behavior.

For each source value, the accumulator is called with the current accumulated state and returns an inner Observable. A new source value cancels the currently active inner before the newly projected inner becomes active.

Each value from the active inner becomes the new accumulated state and is emitted downstream. Cancelled inners can no longer update the state. Source completion waits for the active inner to finish; source, accumulator, or active-inner errors terminate the result.
