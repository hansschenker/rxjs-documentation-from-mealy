# `subscribeOn` Operator Behavior

## Operator Behavior in Plain Language

`subscribeOn` schedules the act of subscribing to the source on the supplied scheduler, optionally after a delay.

The downstream subscription is created immediately, but the source execution does not begin until the scheduled subscription work runs. If the downstream subscription is cancelled before then, the source need not be subscribed at all.

`subscribeOn` changes subscription timing; it does not by itself move every downstream notification onto that scheduler. `observeOn` is the operator for scheduling notification delivery.
