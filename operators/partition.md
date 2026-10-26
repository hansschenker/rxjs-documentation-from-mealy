# `partition` Operator Behavior

## Operator Behavior in Plain Language

`partition` splits one logical source into two Observables according to a predicate.

The first returned Observable emits source values for which the predicate is true. The second emits values for which the predicate is false.

Each returned Observable applies its own filtering subscription to the source unless the source has already been explicitly shared. Source completion and errors flow to each subscribed branch, and predicate errors terminate the affected subscription.
