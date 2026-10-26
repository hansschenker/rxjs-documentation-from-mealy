# `timeoutWith` Operator Behavior

## Operator Behavior in Plain Language

`timeoutWith` is the deprecated convenience form of timeout behavior that switches to a supplied Observable when the timeout expires.

Source values are forwarded while they arrive within the configured timeout period. When the required value does not arrive in time, the source subscription is cancelled and `timeoutWith` subscribes to the fallback Observable.

From then on, the fallback's values and lifecycle notifications are forwarded. Source completion before timeout completes normally; source or fallback errors terminate the result.
