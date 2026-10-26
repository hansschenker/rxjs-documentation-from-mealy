# `throttleTime` Operator Behavior

## Operator Behavior in Plain Language

`throttleTime` applies throttling with a fixed time duration supplied by the scheduler.

With the default configuration, the first source value is emitted immediately and starts a time window during which later source values are suppressed. Those suppressed values do not restart the window.

With trailing enabled, the latest suppressed value can be emitted when the window closes and can begin the next window. Source errors terminate immediately; completion follows the configured leading/trailing policy for any remembered value.
