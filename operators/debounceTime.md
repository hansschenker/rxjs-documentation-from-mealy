# `debounceTime` Operator Behavior

## Operator Behavior in Plain Language

`debounceTime` keeps the latest source value pending until the source has been silent for the configured duration.

Each new source value replaces the pending value and moves the silence deadline forward. Only when no newer value arrives for the full duration is the latest pending value emitted.

If the source completes while a value is pending, that value is emitted before completion. Source errors terminate immediately. The configured scheduler supplies the clock and schedules the waiting period.
