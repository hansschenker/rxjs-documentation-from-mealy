# `delay` Operator Behavior

## Operator Behavior in Plain Language

`delay` shifts source values forward in time by the configured amount.

Each source value is held until its scheduled delivery time and is then forwarded. Delayed values remain ordered according to their scheduled delivery times.

Normal completion waits for delayed values that still need to be delivered, then completes. Source errors terminate rather than being converted into ordinary delayed values. The scheduler supplies the clock and execution mechanism.
