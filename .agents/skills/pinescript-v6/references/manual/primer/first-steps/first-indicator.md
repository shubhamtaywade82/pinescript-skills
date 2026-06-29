# First Indicator

Let's build a simple indicator in Pine Script® v6. This script calculates a 20-period Simple Moving Average (SMA) and plots it on the chart.

### Example Code

```pinescript
//@version=6
indicator("My First SMA Indicator", overlay=true)

// Explicitly define input and MA period
int smaLength = input.int(20, "SMA Length", minval=1)

// Calculate moving average using built-in ta.* namespace
float smaValue = ta.sma(close, smaLength)

// Plot the value on the chart
plot(smaValue, title="20 SMA", color=color.blue, linewidth=2)
```

### Key Elements of the Script
1. `//@version=6`: Specifies that this script compiles under version 6 rules.
2. `indicator(...)`: Defines the script's properties and sets `overlay=true` to display the indicator directly on the price bars.
3. `input.int(...)`: Prompts the user to set a configuration length, defaulting to 20.
4. `ta.sma(...)`: Calculates the moving average series.
5. `plot(...)`: Outputs the calculated series onto the chart.

See also:
- [Next Steps](next-steps.md)
- [Plots](../../visuals/plots.md)
