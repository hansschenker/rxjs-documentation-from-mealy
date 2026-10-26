# `pairwise` Operator Behavior

## Operator Behavior in Plain Language

`pairwise` remembers the immediately previous source value.

The first source value is stored but nothing is emitted because there is no pair yet. For every later source value, `pairwise` emits `[previous, current]` and then makes the current value the new previous value.

Source completion and errors are forwarded. An empty source or a source with only one value produces no pair.
