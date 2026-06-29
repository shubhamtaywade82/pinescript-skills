# Pine Script® v5 to v6 Migration Guide

TradingView provides an automatic conversion tool in the Pine Editor under **Manage Script** -> **Convert code to v6**. However, because v6 introduces strict compiler rules, many scripts require manual adjustments. This guide outlines the breaking changes and how to resolve them.

## 1. Step-by-Step Conversion Flow

1. **Run Auto-Converter**: Use the Pine Editor's built-in converter to automate basic syntax updates.
2. **Change Version Header**: Ensure the script begins with `//@version=6`.
3. **Fix Boolean Types**: Locate all conditions and variables of type `bool` that could be `na` or that rely on implicit numbers.
4. **Fix Strategy Orders**: Remove any `when` parameters from order functions and wrap them in `if` structures.
5. **Fix Object History**: Look for dot-notation history calls like `myObject.field[1]` and rewrite them as `(myObject[1]).field`.

---

## 2. Key Breaking Changes & Fixes

### Boolean Strictness
- **Problem**: In v5, a non-zero integer or float could be used in place of a boolean. In v6, this raises a compilation error.
- **Solution**: Explicitly compare the variable to zero or `na`.

```pinescript
// ❌ v5 (Implicit Boolean)
if volume
    log.info("Volume exists")

// ✅ v6 (Explicit Comparison)
if volume != 0
    log.info("Volume exists")
```

- **Problem**: `na` can no longer be assigned to a `bool`.
- **Solution**: Set the default value of the boolean to `false` or use conditional checks.

---

### Strategy `when` Parameter Removal
- **Problem**: The `when` parameter is removed from all `strategy.*` entry and exit calls.
- **Solution**: Wrap the function call inside an `if` block matching the condition.

```pinescript
// ❌ v5
strategy.entry("Long", strategy.long, when = buySignal)

// ✅ v6
if buySignal
    strategy.entry("Long", strategy.long)
```

---

### Object Field History Operator (`[]`)
- **Problem**: Using `myObject.field[1]` is no longer allowed because the compiler expects the history operator to apply to the object reference itself, or a primitive variable.
- **Solution**: Wrap the object reference in parentheses, apply history to the object, then access the field. Or buffer the field.

```pinescript
// ❌ v5
var myPoint = Point.new(1.23)
plot(myPoint.price[1])

// ✅ v6 (Pattern A)
plot((myPoint[1]).price)

// ✅ v6 (Pattern B)
float currentPrice = myPoint.price
plot(currentPrice[1])
```

---

### Library Exports
- **Problem**: In v5, user-defined types (UDTs) inside a library were exported automatically if they were used in exported functions. In v6, they must be explicitly prefixed with `export`.
- **Solution**: Add the `export` keyword before the `type` declaration in your library.

```pinescript
// ✅ v6 Library code
//@version=6
library("MyCustomLibrary")

export type PivotPoint
    int index
    float price
```
