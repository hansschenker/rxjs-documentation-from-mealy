# `refCount` Operator Behavior

## Operator Behavior in Plain Language

`refCount` manages the connection of a connectable Observable from the number of downstream subscribers.

When the first downstream subscriber arrives, `refCount` connects the connectable to its source. Additional subscribers share that same connection.

As subscribers leave, the reference count falls. When the final subscriber unsubscribes before the shared execution has already terminated, the source connection is disconnected. A later subscriber can therefore establish a new shared connection.
