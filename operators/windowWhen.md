# `windowWhen` Operator Behavior

## Operator Behavior in Plain Language

`windowWhen` maintains one current nested Observable window at a time.

It opens and emits an initial window, then creates a closing Observable from the closing selector. Source values are forwarded into the current window.

When the closing Observable emits or completes, the current window completes, a new window is opened and emitted, and a new closing Observable is created. Source completion completes the current window and outer result. Source, selector, or closing-stream errors terminate the result.
