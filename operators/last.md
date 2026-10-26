# `last` Operator Behavior

## Operator Behavior in Plain Language

`last` remembers the most recent source value that qualifies, using the optional predicate when supplied.

It emits nothing while the source is running because a later qualifying value may replace the current candidate.

When the source completes, the remembered value is emitted and the result completes. If no qualifying value exists, the supplied default is emitted when present; otherwise the result errors with `EmptyError`. Source or predicate errors terminate the result.
