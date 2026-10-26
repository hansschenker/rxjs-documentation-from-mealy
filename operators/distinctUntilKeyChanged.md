# `distinctUntilKeyChanged` Operator Behavior

## Operator Behavior in Plain Language

`distinctUntilKeyChanged` suppresses consecutive source objects whose selected property has not changed.

The first source value is emitted and remembered. Each later value compares its configured property with that property on the previously emitted value, using strict equality by default or the supplied comparator.

If the keys compare equal, the value is suppressed. If they differ, the value is emitted and becomes the new previous value. Source completion and errors are forwarded.
