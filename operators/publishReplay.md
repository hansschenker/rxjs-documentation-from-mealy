# `publishReplay` Operator Behavior

## Operator Behavior in Plain Language

`publishReplay` is a deprecated multicasting operator based on `ReplaySubject`.

It shares one source subscription and stores a configured number or time window of past values. Subscribers can therefore receive buffered past values before receiving new shared values.

Without a selector it returns a connectable Observable that must be connected. With a selector, the replaying shared Observable is scoped to the selector-defined subscription. Source completion and errors are replayed according to the ReplaySubject lifecycle.
