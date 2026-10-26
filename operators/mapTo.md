# `mapTo` Operator Behavior

## Operator Behavior in Plain Language

`mapTo` replaces every source value with the same configured constant value.

Each source `next` therefore produces exactly one downstream `next`, but the original payload is ignored.

Source completion and errors are forwarded unchanged. `mapTo` is deprecated in favor of `map(() => constant)`.
