# `multicast` Operator Behavior

## Operator Behavior in Plain Language

`multicast` is a deprecated multicasting operator that routes one source subscription through a Subject or Subject factory.

Without a selector, it produces a connectable Observable: downstream subscribers attach to the Subject, but the source does not start until the connectable is explicitly connected.

With a selector, the Subject-backed shared Observable is passed into the selector and the resulting dataflow is connected to the source for that subscription. Source and selector lifecycle signals flow through that shared execution.
