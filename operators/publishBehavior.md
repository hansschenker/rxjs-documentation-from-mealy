# `publishBehavior` Operator Behavior

## Operator Behavior in Plain Language

`publishBehavior` is a deprecated multicasting operator based on `BehaviorSubject`.

It creates a connectable Observable whose subscribers share one source subscription after `connect()` is called. Before the source has emitted, subscribers receive the configured initial value; later subscribers receive the current most recent value from the BehaviorSubject.

Source values, completion, and errors are multicast through that subject.
