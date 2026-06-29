# Variable Declarations

Variables store data for reuse. In Pine Script®, variables can be declared using explicit typing or implicit type inference.

### Operators
- `=`: Defines a new variable.
- `:=`: Reassigns an existing variable.

### Modifiers
- `var`: Initializes a variable once on the first bar and persists its value across bars.
- `varip`: Initializes a variable once and persists its value across bars *and* ticks without rollback.

```pinescript
//@version=6
indicator("Variable Declarations Demo")

// Explicit type and assignment
int count = 1

// Reassignment
count := count + 1

// Persistent variable
var float firstClose = close

plot(firstClose)
```

See also:
- [Execution Model](execution-model.md)
- [Type System](type-system.md)
