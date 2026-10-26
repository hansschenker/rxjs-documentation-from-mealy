# `windowTime` Operator Behavior

## Operator Behavior in Plain Language

`windowTime` opens and closes nested Observable windows according to time.

A window stays open for the configured span and receives every source value that arrives while it is active. With a creation interval, new windows can open periodically and overlap; with a maximum window size, a window can close early.

Each new window is emitted immediately from the outer Observable. Source completion completes all active windows and then the outer result. Timing comes from the configured scheduler, and source errors error the active windows and outer result.
