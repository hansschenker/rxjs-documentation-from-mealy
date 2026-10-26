# `max` Operator Behavior

## Operator Behavior in Plain Language

`max` remembers the greatest source value seen so far according to the supplied comparer, or normal JavaScript ordering when no comparer is supplied.

It emits nothing while the source is active because a later value may become the maximum.

When the source completes, the remembered maximum is emitted once and the result completes. With no source values and no seed concept, no maximum can be produced. Source or comparer errors terminate the result.
