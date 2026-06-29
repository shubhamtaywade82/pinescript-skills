# Pine Script® v6 Type System Reference

In Pine Script® v6, the type system has been made significantly stricter and more robust to prevent common runtime bugs and make code evaluation predictable.

## 1. Stricter Boolean Logic

### No `na` for Booleans
In Pine Script v6, boolean variables can only have two states: `true` or `false`. They can no longer be `na`.
- Attempting to assign `na` to a `bool` variable will cause a compiler error or cast warning.
- Because booleans cannot be `na`, you cannot use functions like `na()`, `nz()`, or `fixnan()` with boolean arguments. Doing so will result in compilation errors.

### Removal of Implicit Casting
Implicit casting of numeric types (`int` and `float`) to `bool` is no longer allowed.
- **In v5 and earlier**, numeric values were implicitly treated as boolean inside conditionals (where `0` or `na` was `false`, and non-zero was `true`).
- **In v6**, this is prohibited. You must use explicit comparison operators.

#### Correcting Conditions:
```pinescript
// ❌ INVALID in v6
int x = 5
if x
    strategy.close_all()

//  VALID in v6
int x = 5
if x != 0
    strategy.close_all()
```

## 2. Lazy Evaluation of Logical Operators

The `and` and `or` logical operators now use lazy (short-circuit) evaluation.
- **Lazy `and`**: If the left operand evaluates to `false`, the right operand is not evaluated at all.
- **Lazy `or`**: If the left operand evaluates to `true`, the right operand is not evaluated at all.
- This is highly useful for preventing errors. For example, if you check if an array index exists before accessing it, the second check will not be executed if the first check is false:
```pinescript
// Safe in v6 (no index-out-of-bounds error if array is empty)
if array.size(myArray) > 0 and array.get(myArray, 0) == targetValue
    log.info("Match found")
```

## 3. Type Forms (Qualifiers)

Every variable in Pine Script has a data type and a type qualifier (form). The qualifier determines when the variable's value can change:

| Qualifier | Description |
| :--- | :--- |
| **`const`** | Known at compile time (e.g., literal values, mathematical calculations on constants). |
| **`input`** | Determined in the script settings panel by the user. Remains constant during script execution. |
| **`simple`** | Value is established before script execution starts, but can vary by symbol/timeframe (e.g., symbol inputs). |
| **`series`** | The value can change on every bar of the chart (e.g., `close`, `open`, or values calculated dynamically on each bar). |

In v6, variables initialized with literals (like `5` or `"string"`) default to their most specific qualifiers, but the compiler automatically converts them to broader qualifiers (`series`) where needed.
