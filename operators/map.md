# `map` Operator Behavior

## Operator Behavior in Plain Language

`map` applies a projection function to every source value independently.

Each source value is passed to the projection together with its zero-based index, and the returned value is emitted immediately downstream.

`map` changes the value, not the timing or cardinality of successful source emissions. Source completion and errors are forwarded; an error thrown by the projection terminates the result.
