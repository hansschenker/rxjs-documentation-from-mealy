# `pluck` Operator Behavior

## Operator Behavior in Plain Language

`pluck` reads a configured property path from every source value and emits the value found at that path.

Each source value produces one downstream value. If an intermediate property is missing, the extracted result is `undefined` rather than the original object.

Source completion and errors are forwarded. `pluck` is deprecated in favor of `map` with normal property access.
