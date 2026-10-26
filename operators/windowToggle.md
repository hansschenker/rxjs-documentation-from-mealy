# `windowToggle` Operator Behavior

## Operator Behavior in Plain Language

`windowToggle` can keep several nested Observable windows open at once.

Each emission from the openings Observable creates and emits a new window. The closing selector returns a closing Observable dedicated to that window.

Every source value is sent to every currently open window. When a window's closing Observable emits or completes, that window completes and is removed. Source completion closes all windows and completes the outer result; source, opening, selector, or closing errors terminate the affected result.
