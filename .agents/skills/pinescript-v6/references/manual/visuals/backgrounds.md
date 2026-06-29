# Backgrounds

The `bgcolor()` function changes the color of the script’s background. If the script is running in `overlay = true` mode, it colors the chart’s background.

The function’s signature is:
```pinescript
bgcolor(color, offset, editable, show_last, title, force_overlay) → void
```
Its `color` parameter accepts a "series color" argument, allowing it to be dynamically calculated on every bar. If the color lacks transparency, you can generate one on the fly using `color.new()`.

---

## Session Backgrounds Example

Below is a script that highlights different trading sessions (Pre-market, Regular session, Post-market) on intraday charts. Try it on a 30-minute EURUSD chart:

```pinescript
//@version=6
indicator("Session backgrounds", overlay = true)

// Default color constants using transparency of 25 (hex '40' corresponds to Pine 75% transparency)
BLUE_COLOR   = #0050FF40
PURPLE_COLOR = #0000FF40
PINK_COLOR   = #5000FF40
NO_COLOR     = color(na)

// Allow user to change the colors
preMarketColor  = input.color(BLUE_COLOR, "Pre-market")
regSessionColor = input.color(PURPLE_COLOR, "Regular Session")
postMarketColor = input.color(PINK_COLOR, "Post-market")

// Function returns true if the bar time falls in the session range
timeInRange(tf, session) => 
    not na(time(tf, session))

// Function prints a message at the bottom-right of the chart
f_print(_text) => 
    var table _t = table.new(position.bottom_right, 1, 1)
    table.cell(_t, 0, 0, _text, bgcolor = color.yellow)

var chartIs30MinOrLess = timeframe.isseconds or (timeframe.isintraday and timeframe.multiplier <= 30)
sessionColor = if chartIs30MinOrLess
    switch
        timeInRange(timeframe.period, "0400-0930") => preMarketColor
        timeInRange(timeframe.period, "0930-1600") => regSessionColor
        timeInRange(timeframe.period, "1600-2000") => postMarketColor
        => NO_COLOR
else
    f_print("No background is displayed.\nChart timeframe must be <= 30min.")
    NO_COLOR

bgcolor(sessionColor)
```

### Key Implementation Details:
*   **Timeframe constraint**: The script only executes sessions on timeframes ≤ 30 minutes. If the timeframe is higher, it displays an error notification using a table cell.
*   **Base colors**: The color variables use hex transparency suffixing (`40` in hex translates to `75` on the decimal `0-100` Pine Script scale).
*   **Default return**: The `else` block and switch default return `NO_COLOR` (`color(na)`) so the background remains transparent on off-session bars.

---

## Dynamic Gradient Backgrounds (CCI Rank)

In this example, we generate a smooth background color gradient based on the percentile rank of the Commodity Channel Index (CCI) indicator:

```pinescript
//@version=6
indicator("CCI Background")

bullColor = input.color(color.lime, "Bullish Gradient Color", inline = "1")
bearColor = input.color(color.fuchsia, "Bearish Gradient Color", inline = "1")

// Calculate CCI
myCCI = ta.cci(hlc3, 20)

// Get relative position of CCI in last 100 bars on a 0-100% scale
myCCIPosition = ta.percentrank(myCCI, 100)

// Generate a bull gradient when position is 50-100%, bear gradient when position is 0-50%
backgroundColor = if myCCIPosition >= 50
    color.from_gradient(myCCIPosition, 50, 100, color.new(bullColor, 75), bullColor)
else
    color.from_gradient(myCCIPosition, 0, 50, bearColor, color.new(bearColor, 75))

// Plot the CCI line using a dual-plot outline for high visibility contrast
plot(myCCI, "CCI Border", color.white, 3)
plot(myCCI, "CCI Center", color.black, 1)

// Reference zero line
hline(0, "Zero Level", color = color.gray)

// Apply gradient background
bgcolor(backgroundColor)
```

### Key Implementation Details:
*   **Rank calculations**: `ta.percentrank()` measures what percentage of the last 100 bars had a lower CCI value than the current bar.
*   **Color interpolation**: `color.from_gradient()` determines the background color by interpolating between the raw color and its transparent version depending on the percent rank.
*   **Inline inputs**: Both color input fields are grouped on the same line in the "Inputs" panel using `inline = "1"`.
*   **Dual-plot outlining**: Plotting the CCI line twice (a thick white line underneath a thin black line) guarantees that it remains highly readable over a changing color background.
