# `onErrorResumeNext` Operator Behavior

## Operator Behavior in Plain Language

`onErrorResumeNext` forwards values from the source and then continues through the supplied Observable inputs in sequence.

When the current Observable completes normally, it moves to the next one. When the current Observable errors, that error is suppressed and it also moves to the next one instead of terminating.

The result completes after every supplied Observable has completed or errored. It is therefore sequential continuation that treats errors as a signal to continue, not as terminal output.
