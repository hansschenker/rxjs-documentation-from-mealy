# `expand` Operator Behavior

## Operator Behavior in Plain Language

`expand` recursively feeds emitted values back through a projection function to create more Observable work.

Each source value is emitted downstream and is also projected to an inner Observable. Values produced by those projected Observables are themselves emitted and recursively projected again.

Several projected branches may be active concurrently, subject to the configured concurrency limit. Completion occurs when the original source is complete and no projected work remains. Source, projection, or inner errors terminate the recursive expansion.
