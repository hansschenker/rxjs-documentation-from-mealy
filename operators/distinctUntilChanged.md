# `distinctUntilChanged` Operator Behavior

## Operator Behavior in Plain Language

`distinctUntilChanged` compares each source value only with the most recently emitted value.

The first value is always emitted and becomes the remembered previous value. For each later value, an optional key selector derives comparison keys and the comparator decides whether the previous emitted key and current key are equal.

Equal values are suppressed. A different value is emitted and becomes the new remembered value. Source completion and errors are forwarded; selector or comparator errors terminate the result.
