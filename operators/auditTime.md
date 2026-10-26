# `auditTime` Operator Behavior

## Operator Behavior in Plain Language

`auditTime` starts with no audit window active.

When a source value arrives while no window is active, it remembers that value and starts the configured duration on the scheduler. Later values during that duration replace the remembered value without restarting the timer.

When the duration ends, `auditTime` emits the latest remembered value and becomes ready for another window.

If the source completes while a value is pending, the pending value is emitted when the current duration ends and the result then completes. Source errors terminate the result.
