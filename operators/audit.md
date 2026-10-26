# `audit` Operator Behavior

## Operator Behavior in Plain Language

`audit` starts without an active duration window.

When a source value arrives while no duration is active, `audit` remembers that value and creates a duration Observable from it. While that duration is active, later source values replace the remembered value but do not restart the duration.

When the duration emits or completes, `audit` emits the most recently remembered source value and becomes ready for a new duration window.

If the source completes while a value is pending, `audit` waits for the active duration to end, emits the pending value, and then completes. Source, selector, or duration errors terminate the result.
