# Bar Coloring

The `barcolor()` function colors the bars (candles) of the main chart series. It behaves this way regardless of whether the script is configured to display in the main chart pane (`overlay = true`) or in a separate indicator pane (`overlay = false`).

The function’s signature is:
```pinescript
barcolor(color, offset, editable, show_last, title, display) → void
```
The `color` parameter accepts a "series color" argument, enabling you to calculate and apply candle colors conditionally based on your script's logic.

---

## Inside and Outside Bar Coloring Example

Below is a script that highlights inside bars and outside bars with distinct colors:

```pinescript
//@version=6
indicator("barcolor example", overlay = true)

isUp = close > open
isDown = close <= open

// Define conditions for inside and outside bars
isOutsideUp   = high > high[1] and low < low[1] and isUp
isOutsideDown = high > high[1] and low < low[1] and isDown
isInside      = high < high[1] and low > low[1]

// Conditionally color the bars on the main chart
barcolor(isInside ? color.yellow : isOutsideUp ? color.aqua : isOutsideDown ? color.purple : na)
```

### Key Implementation Details:
*   **Leaving default colors intact**: Setting the color argument to `na` instructs the Pine Script engine to leave those candles colored in the user's default chart template.
*   **Nested Ternary Expressions**: The nested `?:` ternary operators determine which color to return to the `barcolor()` call based on which condition is evaluated as true.
