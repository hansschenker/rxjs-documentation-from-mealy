# `distinct` Operator Behavior

## Operator Behavior in Plain Language

`distinct` remembers every comparison key that has already been accepted.

For each source value it derives a key, using the key selector when supplied. If that key has not been seen, the key is stored and the original value is emitted; if it has been seen, the value is suppressed.

When a flushes Observable is supplied, each flush emission clears the remembered key set so old keys may pass again. Source, selector, or flush errors terminate the result.
