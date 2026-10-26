# `single` Operator Behavior

## Operator Behavior in Plain Language

`single` verifies that exactly one source value satisfies the optional predicate.

A qualifying value is remembered, but it is not emitted immediately because a later qualifying value would violate the uniqueness requirement. If a second qualifying value arrives, `single` errors immediately.

When the source completes with exactly one qualifying value, that value is emitted and the result completes. If there is no qualifying value, the result errors. Source or predicate errors terminate the result.
