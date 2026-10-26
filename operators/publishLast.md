# `publishLast` Operator Behavior

## Operator Behavior in Plain Language

`publishLast` is a deprecated multicasting operator based on `AsyncSubject`.

Subscribers share one source subscription after the connectable is connected. Source values are remembered but are not emitted immediately by the AsyncSubject.

When the source completes normally, only the final source value is emitted to subscribers and then the shared stream completes. If the source errors, the error is forwarded instead of a final value.
