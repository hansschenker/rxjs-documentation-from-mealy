# `delayWhen` Operator Behavior

## Operator Behavior in Plain Language

`delayWhen` chooses a separate delay for every source value.

For each value, the delay-duration selector returns an Observable associated with that value. The value is held until its duration Observable emits or completes, then it is released downstream.

Several values can be pending simultaneously, so later source values can be released before earlier ones when their durations finish sooner. The result completes after the source is complete and all pending values have been released. Source, selector, or duration errors terminate the result.
