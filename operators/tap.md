# `tap` Operator Behavior

## Operator Behavior in Plain Language

`tap` mirrors the source while running user-supplied side-effect callbacks around the subscription lifecycle and notifications.

For each source value, the appropriate `next` callback runs and the original value is then forwarded unchanged. Error and completion callbacks observe the matching lifecycle signal without replacing it.

`tap` can also observe subscription, explicit unsubscription, and finalization through the supported observer callbacks. If a callback invoked by `tap` throws during notification handling, that error becomes an error of the resulting Observable.
