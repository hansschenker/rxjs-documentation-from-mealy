# `retryWhen` Operator Behavior

## Operator Behavior in Plain Language

`retryWhen` turns source errors into signals that can decide whether another source subscription should start.

Source values are forwarded normally. When the source errors, that error is sent to the errors Observable supplied to the user-defined notifier function instead of immediately terminating downstream.

Each notifier emission causes a fresh subscription to the source. If the notifier completes, the result completes; if it errors, the result errors. A normal source completion completes normally and does not invoke retry behavior.
