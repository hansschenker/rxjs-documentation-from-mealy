# `bufferTime` Operator Behavior

## Operator Behavior in Plain Language

`bufferTime` collects source values into arrays whose opening and closing are controlled by time.

A buffer remains active for the configured span, and every source value arriving while it is open is added to it. With a creation interval, several buffers can overlap; with a maximum size, a buffer can close early.

When a buffer closes, it is emitted as an array. On source completion, all active buffers are emitted and the result completes. Timing is provided by the configured scheduler; source errors terminate the result.
