# `repeat` Operator Behavior

## Operator Behavior in Plain Language

`repeat` resubscribes to the source after normal completion.

Values from each source execution are forwarded as they occur. When one execution completes, `repeat` either completes because the configured repeat count has been exhausted, or waits for the configured delay/notifier and then starts a fresh source subscription.

Errors are not repeated: a source error is forwarded immediately. A delay notifier can trigger the next subscription, complete to stop repeating, or error to terminate the result.
