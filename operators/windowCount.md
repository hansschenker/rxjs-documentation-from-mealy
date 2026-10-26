# `windowCount` Operator Behavior

## Operator Behavior in Plain Language

`windowCount` opens windows according to source-value counts.

A window collects source values until it reaches the configured size, at which point that window completes. New windows start every configured start interval; therefore windows may be back-to-back, overlap, or leave gaps.

Each opened window is emitted immediately as a nested Observable. On source completion, all open windows complete and the outer result completes. Source errors error the open windows and outer result.
