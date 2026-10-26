# `reduce` Operator Behavior

## Operator Behavior in Plain Language

`reduce` accumulates source values into one state but emits no intermediate states.

With a seed, accumulation starts from that seed. Without a seed, the first source value becomes the initial accumulated state and later values are passed to the accumulator with the current state and source index.

When the source completes, the final accumulated state is emitted once and the result completes. If there was no seed and no source value, there is no accumulated value to emit. Source or accumulator errors terminate the result.
