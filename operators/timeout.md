# `timeout` Operator Behavior

## Operator Behavior in Plain Language

`timeout` watches whether source values arrive within configured deadlines.

A `first` deadline can constrain only the first source value. An `each` duration can constrain the gap between later values and, when no separate `first` setting is supplied, can also constrain the first value.

Each timely source value is forwarded and updates the timeout state. If a deadline expires first, the source subscription is cancelled. By default the result errors with `TimeoutError`; when a `with` factory is configured, `timeout` switches to that replacement Observable instead.

Normal source completion completes the result, and source or replacement errors are forwarded. The configured scheduler supplies the clock.
