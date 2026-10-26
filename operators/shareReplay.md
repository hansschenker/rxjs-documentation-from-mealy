# `shareReplay` Operator Behavior

## Operator Behavior in Plain Language

`shareReplay` shares one source execution and replays buffered past values to later subscribers through a `ReplaySubject`.

The configured buffer size and time window determine what history a new subscriber receives before current shared values. A successfully completed source remains cached, so later subscribers can receive the replayed values and completion without rerunning the source.

After an error the shared state resets so a later subscriber can create a new source execution. By default `refCount` is false, so the source can remain connected even with no subscribers; with `refCount: true`, dropping to zero disconnects and resets the shared execution.
