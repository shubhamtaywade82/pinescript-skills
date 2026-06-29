# User-Defined Functions

You can create custom functions (User-Defined Functions) to encapsulate reusable math or logic.

### Syntax
```pinescript
//@version=6
indicator("UDF Demo")

// Declare user-defined function
calculateMultiplier(float basePrice, int multiplier) =>
    basePrice * multiplier

// Use function
float result = calculateMultiplier(close, 2)
plot(result)
```

See also:
- [Built-ins](built-ins.md)
- [Methods](methods.md)
