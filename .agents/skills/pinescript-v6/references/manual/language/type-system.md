# Type System

Pine Script® v6 uses a type system that combines **types** (categories of data) and **qualifiers** (when values are determined and whether they can change across bars).

### Type Qualifiers (Strictest to Loosest)
1. `const`: Value known at compile time (e.g., literal numbers or strings).
2. `input`: Value known at input/settings time (returned by `input.*()` functions).
3. `simple`: Value determined on the first historical bar and constant thereafter.
4. `series`: Value that can change dynamically on any bar (e.g., `close`, `high`).

### Value Types
- `int`, `float`, `bool`, `color`, `string`
- Custom object/UDT, Enum, and collection types (`array`, `matrix`, `map`).

> [!IMPORTANT]
> In Pine Script® v6, boolean evaluations are strict. Numeric values cannot be implicitly coerced into booleans. E.g., `if close` is invalid; you must write `if close > 0` or similar explicit comparison.

See also:
- [Type System Rules](../../type-system.md)
- [Variable Declarations](variable-declarations.md)
