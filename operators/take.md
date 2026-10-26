# `take` Operator Behavior

## Operator Behavior in Plain Language

`take` forwards only the first configured number of source values.

Each source value is counted and emitted while the count remains within the requested amount. As soon as the requested number of values has been emitted, `take` completes immediately and unsubscribes from the source.

If the source completes earlier, the result simply completes with however many values were available. A non-positive count produces an empty result without needing source values. Source errors before completion are forwarded.
