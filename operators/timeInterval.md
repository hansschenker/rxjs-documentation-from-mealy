# `timeInterval` Operator Behavior

## Operator Behavior in Plain Language

`timeInterval` measures elapsed scheduler time between consecutive source emissions.

At subscription time it records the starting time. When the first source value arrives, it emits that value together with the time since subscription. For each later source value, it emits the value with the time since the previous source emission.

The scheduler supplies the clock. Source completion and errors are forwarded after the corresponding interval values already emitted.
