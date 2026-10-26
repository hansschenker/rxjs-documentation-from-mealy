# `concatAll` Operator Behavior

## Operator Behavior in Plain Language

`concatAll` receives inner Observables from an outer Observable and processes them one at a time.

When an inner arrives and none is active, it is subscribed immediately. Additional inners arriving while one is active are queued in arrival order.

Values from the active inner are forwarded. When it completes, the next queued inner starts. After the outer completes, active and queued inners are still allowed to finish; the result completes only when the outer is complete, no inner is active, and the queue is empty. Any outer or inner error terminates the result.
