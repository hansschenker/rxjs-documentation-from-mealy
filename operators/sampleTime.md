# `sampleTime` Operator Behavior

## Operator Behavior in Plain Language

`sampleTime` periodically checks for the latest source value using the configured scheduler and period.

Source emissions update the remembered latest value but do not emit immediately. At each sampling tick, the latest value is emitted only if at least one new source value has arrived since the previous sample.

The result completes when the source completes. Source errors terminate immediately. The scheduler supplies the periodic clock.
