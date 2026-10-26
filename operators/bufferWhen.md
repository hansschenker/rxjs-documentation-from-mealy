# `bufferWhen` Operator Behavior

## Operator Behavior in Plain Language

`bufferWhen` starts by opening an empty buffer and creating a closing Observable from the closing selector.

Source values are collected into the current buffer. When the current closing Observable emits or completes, `bufferWhen` emits that buffer, opens a new empty buffer, and calls the selector again for the next closing Observable.

The cycle repeats until the source completes. On source completion, the current buffer is emitted and the result completes. Source, selector, or closing-stream errors terminate the result.
