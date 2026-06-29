# Levels

Horizontal levels can be drawn across the chart pane using the `hline()` function. This function is designed to plot horizontal levels using a single color that does not change across bars.

The function signature is:
```pinescript
hline(price, title, color, linestyle, linewidth, editable, display) → hline
```

## Constraints of `hline()`

Compared to the standard `plot()` function, `hline()` has a few compile-time constraints:
1.  **Static Price**: The `price` parameter requires an "input int/float" argument. You cannot pass "series float" values (like `close`) or dynamic calculations.
2.  **Static Color**: The `color` parameter requires an "input color" argument. Dynamic colors calculated bar-by-bar cannot be used.
3.  **Line Styles**: Three built-in styles are supported via the `linestyle` parameter:
    *   `hline.style_solid`
    *   `hline.style_dotted`
    *   `hline.style_dashed`

---

## True Strength Index (TSI) Level Example

This script plots the TSI indicator along with 5 horizontal boundary bands:

```pinescript
//@version=6
indicator("TSI")

// Calculate TSI and rescale to typical +100 to -100 bounds
myTSI = 100 * ta.tsi(close, 25, 13)

// Plot levels using hline
hline( 50, "+50",  color.lime)
hline( 25, "+25",  color.green)
hline(  0, "Zero", color.gray, linestyle = hline.style_dotted)
hline(-25, "-25",  color.maroon)
hline(-50, "-50",  color.red)

plot(myTSI, "TSI Line")
```

---

## Shading the Space Between Levels

The space between two levels plotted with `hline()` can be filled with background colors using the `fill()` function. Both IDs passed to `fill()` must be of the `hline` type.

Here is the modified TSI script featuring colored background zones:

```pinescript
//@version=6
indicator("TSI")

myTSI = 100 * ta.tsi(close, 25, 13)

// Store the return values (hline IDs) into variables
plus50Hline  = hline( 50, "+50",  color.lime)
plus25Hline  = hline( 25, "+25",  color.green)
zeroHline    = hline(  0, "Zero", color.gray, linestyle = hline.style_dotted)
minus25Hline = hline(-25, "-25",  color.maroon)
minus50Hline = hline(-50, "-50",  color.red)

// Helper function to return a light background shade with 90% transparency
fillColor(color col) =>
    color.new(col, 90)

// Shading the space between sequential hline zones
fill(plus50Hline,  plus25Hline,  fillColor(color.lime),  "Extreme Bull Fill")
fill(plus25Hline,  zeroHline,    fillColor(color.teal),  "Bull Zone Fill")
fill(zeroHline,    minus25Hline, fillColor(color.maroon), "Bear Zone Fill")
fill(minus25Hline, minus50Hline, fillColor(color.red),    "Extreme Bear Fill")

plot(myTSI, "TSI Line")
```

### Key Implementation Details:
*   **hline ID references**: We capture the return value of each level as a variable (e.g. `plus50Hline` is of the `hline` type) to use them in the `fill()` function.
*   **Visual contrast**: The `fillColor` helper function ensures the fills remain transparent (`90` transparency) so that the primary plots and price bars remain visible.
*   **Palette choices**: We use `color.teal` instead of `color.green` for the bull zone fill to match the dark and light chart themes more harmoniously.
