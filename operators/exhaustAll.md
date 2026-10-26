# `exhaustAll` Operator Behavior

## Operator Behavior in Plain Language

`exhaustAll` receives inner Observables and subscribes to an inner only when no other inner is active.

While an inner is active, newly arriving inners are ignored rather than queued or used to replace it. Values from the accepted inner are forwarded until it completes.

After that completion, a later arriving inner may become active. Outer completion waits for the current inner to finish. Outer or active-inner errors terminate the result.
