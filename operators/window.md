# `window` Operator Behavior

## Operator Behavior in Plain Language

`window` partitions source values into nested Observables instead of arrays.

It starts with one open window and emits that window from the outer result. Source values are forwarded into the current window.

Whenever the boundary Observable emits, the current window completes and a new window is opened and emitted. Source completion completes the current window and the outer result. Errors from the source or boundary Observable error the active window and outer result.
