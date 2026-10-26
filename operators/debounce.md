# `debounce` Operator Behavior

## Operator Behavior in Plain Language

`debounce` keeps at most one pending source value.

Each source value replaces the pending value and creates a new duration Observable. If another source value arrives before that duration signals, the previous duration subscription is cancelled and a new duration is created for the new value.

When the current duration emits or completes, the pending value is emitted and cleared. If the source completes while a value is pending, that value is emitted immediately before completion. Source, selector, or duration errors terminate the result.
