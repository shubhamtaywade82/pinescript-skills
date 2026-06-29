# Script Structure

Every Pine Script® must adhere to a specific structural layout.

### Essential Structure
1. **Compiler Annotation**: `//@version=6` (declares version).
2. **Declaration Statement**: One of `indicator()`, `strategy()`, or `library()`.
3. **Inputs/Logic/Calculations**: Declaring variables and computing indicators/rules.
4. **Outputs**: Outputting results via `plot()`, `plotshape()`, `barcolor()`, `log.info()`, or library exports.

```pinescript
//@version=6
indicator("Minimal Script Structure", overlay=true)

// Calculation
float priceChange = ta.change(close)

// Output
plot(priceChange > 0 ? close : na, color=color.green, style=plot.style_linebr)
```

See also:
- [Declaration Statements](declaration-statements.md)
- [Identifiers](identifiers.md)
