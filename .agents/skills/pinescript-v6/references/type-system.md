# Pine Script® v6 Type System Reference

Source: https://www.tradingview.com/pine-script-docs/language/type-system/
Status: unverified

## Core Overview

Pine Script v6 uses **types** to categorize data and determine compatible operations and functions, and **type qualifiers** to indicate when values are accessible and whether they can change across executions. A qualified type combines both, and this system is tightly linked to the execution model and time series structure.

## Type Qualifiers

Qualifiers follow a strict hierarchy:

```
const < input < simple < series
```

| Qualifier | Timing | Mutability | Compatibility | Key Details |
|-----------|--------|------------|---------------|-------------|
| `const` | Compile time | Constant across all executions | Accepts only `const` values | All literals and `const`-only expressions inherit this qualifier. The `const` keyword prevents reassignment, even to other constant values. |
| `input` | Input time (when user confirms inputs/chart settings) | Constant at runtime | Accepts `const` and `input` values | `input.*()` returns `input` values. Changing an input reloads the script. `chart.*` chart-state variables are `input`. |
| `simple` | Runtime, first available bar only | Constant across all subsequent bars | Accepts `const`, `input`, and `simple` values | Most `syminfo.*` and `timeframe.*` values are `simple`. |
| `series` (strongest) | Runtime | Dynamic, only qualifier that can change across bars | Accepts all qualifier values | All bar data variables and runtime calculation results are `series`. Any expression that can vary per bar inherits this qualifier. |

### Qualifier Rules

1. A parameter or operation accepting a qualifier allows values with weaker qualifiers, but not stronger ones.
2. Expression results inherit the strongest qualifier used in the calculation.
3. Qualifiers cannot be downgraded to make values compatible with stricter operations.

### Common Pitfall

```pine
//@version=6
var string scriptTitle = "My indicator for " + syminfo.ticker // simple string
indicator(title = scriptTitle) // ERROR: title requires const string
```

## Types

### Value Types

| Type | Description | Notes |
|------|-------------|-------|
| `int` | Whole numbers | Literals: `1`, `-1`, `750` |
| `float` | Floating-point numbers | Internal precision: `1e-16` |
| `bool` | Boolean values | Never returns `na` |
| `color` | RGB colors | Format: `#RRGGBB` or `#RRGGBBAA` |
| `string` | Text sequences | Concatenate with `+` / `+=` |
| Enum types | Named sets of distinct constant members | Use `input.enum()` for dropdown inputs |

### Reference Types

Reference types hold references (IDs) to objects in memory, always inherit `series`, and are incompatible with arithmetic/logical operators. Examples include collection types, drawing types, `plot`, and UDTs created with `type`.

## When Explicit Types Are Required

1. Declaring variables, UDF parameters, or UDT fields with initial `na` values.
2. Defining parameters of exported library functions or declared exported constants.
3. Using qualifier keywords in variable or parameter declarations.
4. Declaring the first parameter of a user-defined method.
