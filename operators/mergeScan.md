# `mergeScan` Operator Behavior

## Operator Behavior in Plain Language

`mergeScan` combines accumulation with concurrent inner Observables.

For each accepted source value, the accumulator is called with the current accumulated state and returns an inner Observable. Inner subscriptions may overlap up to the configured concurrency limit.

Every value emitted by an active inner becomes the new accumulated state and is also emitted downstream. Because inners may overlap, state can be updated by whichever active inner emits next. Source completion waits for active and queued work; source, accumulator, or inner errors terminate the result.
