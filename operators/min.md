# `min` Operator Behavior

## Operator Behavior in Plain Language

`min` remembers the smallest source value seen so far according to the supplied comparer, or normal JavaScript ordering when no comparer is supplied.

It emits nothing while the source is active because a later value may become the minimum.

When the source completes, the remembered minimum is emitted once and the result completes. With no source values, no minimum can be produced. Source or comparer errors terminate the result.
