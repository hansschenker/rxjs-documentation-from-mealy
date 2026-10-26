# `sequenceEqual` Operator Behavior

## Operator Behavior in Plain Language

`sequenceEqual` compares two Observables as ordered sequences rather than comparing only their latest values.

It waits for values from both sides, compares corresponding positions with the supplied comparator or strict equality, and emits nothing while equality is still undecided.

The first unequal pair causes `false` to be emitted and the result completes. If one sequence ends with unmatched values, the result is also `false`. Only when both sequences complete with the same number of pairwise-equal values does it emit `true` and complete. Errors terminate the comparison.
