# `scan` Operator Behavior

## Operator Behavior in Plain Language

`scan` maintains accumulated state and emits the new state after each source value.

With a seed, the first source value is combined with that seed. Without a seed, the first source value becomes the accumulated state and is emitted as that state; later values are passed to the accumulator with the current state and index.

Every successful accumulation produces the next downstream value, so the state evolves as a stream. Source completion completes the result, and source or accumulator errors terminate it.
