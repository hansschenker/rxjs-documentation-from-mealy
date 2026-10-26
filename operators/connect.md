# `connect` Operator Behavior

## Operator Behavior in Plain Language

`connect` creates a shared source connection for the dataflow returned by a selector.

For each downstream subscription, a connector Subject is created and an Observable view of it is passed to the selector. The downstream subscriber listens to the selector's result, while one subscription connects the connector to the source.

Source values are therefore multicast inside that selector-defined dataflow. The connection is scoped to that resulting subscription rather than becoming a permanent global share. Source or selector errors terminate the result.
