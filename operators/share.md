# `share` Operator Behavior

## Operator Behavior in Plain Language

`share` turns one source execution into a reference-counted shared execution through a connector Subject.

The first downstream subscriber causes a source subscription to be created. Further subscribers receive values from that same Subject-backed execution rather than creating independent source executions.

When subscribers leave, the reference count is updated. By default the shared state resets when the source errors, completes, or the reference count falls to zero, although `ShareConfig` can change those reset policies and the connector type.

Source values and lifecycle notifications are multicast to the current subscribers.
