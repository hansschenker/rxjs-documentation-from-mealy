# `repeatWhen` Operator Behavior

## Operator Behavior in Plain Language

`repeatWhen` turns source completion into a signal that can decide whether another source subscription should start.

Source values are forwarded normally. When the source completes, a completion notification is sent to the notifier Observable created by the user-supplied notifier function.

Each notifier emission causes a fresh subscription to the source. If the notifier completes, the result completes; if it errors, the result errors. Source errors are not converted into repeat signals and are forwarded immediately.
