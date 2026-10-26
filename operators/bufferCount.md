# `bufferCount` Operator Behavior

## Operator Behavior in Plain Language

`bufferCount` groups source values into arrays according to a maximum buffer size and an optional interval for starting new buffers.

With only a buffer size, one buffer is active at a time and is emitted when it reaches that size. With a start interval, new buffers open every configured number of source values, so buffers may overlap or leave gaps.

Each buffer is emitted when full. When the source completes, any non-empty buffers still open are emitted before completion. A source error terminates without flushing incomplete buffers.
