# `retry` Operator Behavior

## Operator Behavior in Plain Language

`retry` forwards values from the current source subscription, but treats a source error as a possible request to resubscribe.

If retry capacity remains, the failed subscription is torn down and a fresh subscription to the source is started immediately or after the configured delay. Values emitted before a failed attempt remain part of the output; a later attempt starts the source again from its own beginning.

When the retry count is exhausted, the error is forwarded. Normal source completion completes immediately. With a notifier delay, notifier emission retries, notifier completion completes the result, and notifier error errors the result.
