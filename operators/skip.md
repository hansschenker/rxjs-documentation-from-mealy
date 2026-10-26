# `skip` Operator Behavior

## Operator Behavior in Plain Language

`skip` suppresses the first configured number of source values.

It counts source emissions from zero. While the count is below the skip amount, values are dropped; after the threshold is reached, every later source value is forwarded unchanged.

Source completion and errors are forwarded. The skipped values are not buffered for later use.
