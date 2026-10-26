# `throttle` Operator Behavior

## Operator Behavior in Plain Language

`throttle` opens a suppression duration from an accepted source value and controls whether leading and trailing values are emitted.

With the default configuration, the first value is emitted immediately and starts a duration; source values arriving while that duration is active are ignored. The duration is not restarted by those values.

With trailing enabled, the latest value seen during the duration is remembered and may be emitted when the duration ends; that trailing emission can start the next throttle period. Source completion, selector behavior, and the leading/trailing configuration determine whether a remembered trailing value is emitted before completion. Errors terminate the result.
