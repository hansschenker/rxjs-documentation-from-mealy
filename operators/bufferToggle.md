# `bufferToggle` Operator Behavior

## Operator Behavior in Plain Language

`bufferToggle` can keep several buffers open at once.

Each emission from the openings Observable creates a new empty buffer. The closing selector is called for that opening value, and the returned Observable controls only that buffer.

Every source value is copied into every currently open buffer. When a buffer's closing Observable emits or completes, that buffer is emitted and removed.

Source completion flushes all open buffers and completes. Errors from the source, openings stream, selector, or closing streams terminate the result.
