# `timestamp` Operator Behavior

## Operator Behavior in Plain Language

`timestamp` attaches the scheduler's current time to every source value.

Each source `next` produces one object containing the original value and a timestamp obtained from the configured scheduler at delivery time.

The operator does not delay or suppress values. Source completion and errors are forwarded unchanged.
