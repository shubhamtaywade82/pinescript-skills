# Operators

Pine Script® supports standard arithmetic, comparison, logical, history, and ternary operators.

### Key Operators
- **Arithmetic**: `+`, `-`, `*`, `/`, `%`
- **Comparison**: `==`, `!=`, `<`, `>`, `<=`, `>=`
- **Logical**: `and`, `or`, `not` (lazy-evaluated in version 6).
- **History Referencing**: `[]` (e.g., `close[1]` represents the close of the previous bar).
- **Ternary**: `condition ? valueIfTrue : valueIfFalse`

```pinescript
//@version=6
indicator("Operators Demo")
float diff = close - open
bool isGreen = diff > 0
float greenClose = isGreen ? close : na
plot(greenClose)
```

See also:
- [Conditional Structures](conditional-structures.md)
