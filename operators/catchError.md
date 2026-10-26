# `catchError` Operator Behavior

## Operator Behavior in Plain Language

`catchError` forwards source values unchanged while the source runs normally.

If the source errors, it unsubscribes from that failed execution and calls the selector with the error and the caught Observable. The selector returns another Observable input, and `catchError` subscribes to it as the replacement.

Values, completion, and errors from the replacement are then forwarded downstream. If the selector throws, that error is forwarded. Normal source completion does not invoke the selector.
