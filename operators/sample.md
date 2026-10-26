# `sample` Operator Behavior

## Operator Behavior in Plain Language

`sample` remembers the latest source value that has arrived since the previous sample.

Source emissions update that remembered value but do not themselves trigger output. Each emission from the notifier asks for a sample: if a new source value has arrived since the previous sample, the latest one is emitted and the pending-sample flag is cleared.

Notifier emissions with no new source value produce nothing. The result completes when the source completes; source or notifier errors terminate the result.
