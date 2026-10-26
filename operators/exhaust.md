# `exhaust` Operator Behavior

## Operator Behavior in Plain Language

`exhaust` receives inner Observables and allows only the first inner that arrives while no inner is active.

While that inner is active, every additional inner emitted by the outer Observable is ignored completely; there is no queue and no cancellation of the active inner.

When the active inner completes, the next inner that arrives may be accepted. If the outer completes while an inner is active, the result waits for that inner to finish. Outer or active-inner errors terminate the result.
