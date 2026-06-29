# Bar Plotting

The `plotcandle()` built-in function is used to plot candles on the chart, while `plotbar()` is used to plot conventional bars.

Both functions require four primary series arguments representing the OHLC prices (open, high, low, close) of the candles or bars being plotted. If any of these four arguments is `na` on a given bar, no visual bar is plotted.

---

## Plotting Candles with `plotcandle()`

The signature of `plotcandle()` is:
```pinescript
plotcandle(open, high, low, close, title, color, wickcolor, editable, show_last, bordercolor, display) → void
```

### 1. Basic Single-Color Candles
This plots simple blue candles using the standard chart OHLC values in a separate pane:
```pinescript
//@version=6
indicator("Single-color candles")
plotcandle(open, high, low, close)
```

### 2. Dual-Color Smoothed Candles
You can build candles using smoothed or calculated averages rather than raw prices:
```pinescript
//@version=6
indicator("Smoothed candles", overlay = true)

lenInput = input.int(9, "Smoothing Length")

// Helper function to smooth price series
smooth(source, length) =>
    ta.sma(source, length)

o = smooth(open, lenInput)
h = smooth(high, lenInput)
l = smooth(low, lenInput)
c = smooth(close, lenInput)

// Dynamic wick coloring based on raw price position relative to smoothed close
ourWickColor = close > c ? color.green : color.red

plotcandle(o, h, l, c, wickcolor = ourWickColor)
```

### 3. Higher-Timeframe (HTF) Daily Candles on Intraday Charts
In this example, daily bars are fetched and plotted directly on top of an intraday chart:
```pinescript
// NOTE: Use this script on an intraday chart.
//@version=6
indicator("Daily bars", behind_chart = false, overlay = true)

// Use gaps to return data only when the 1D timeframe completes, and to return na otherwise.
[o, h, l, c] = request.security(syminfo.tickerid, "D", [open, high, low, close], gaps = barmerge.gaps_on)

const color UP_COLOR = color.silver
const color DN_COLOR = color.blue

color wickColor = c >= o ? UP_COLOR : DN_COLOR
color bodyColor = c >= o ? color.new(UP_COLOR, 70) : color.new(DN_COLOR, 70)

// Plot daily candles only on intraday chart timeframes and only when a daily bar completes
plotcandle(timeframe.isintraday ? o : na, h, l, c, color = bodyColor, wickcolor = wickColor)
```

#### Key Implementation Details:
*   **Layer stacking**: Setting `behind_chart = false` in the script header positions these custom candles in front of the primary chart candles.
*   **Intraday restriction**: Daily candles are hidden if the chart is running on a timeframe greater than or equal to 1D (`timeframe.isintraday ? o : na`).
*   **Gap handling**: By requesting data with `gaps = barmerge.gaps_on`, the script receives `na` on intrabars and only plots the completed daily candle once per day.
*   **Tuple requesting**: The script requests `[open, high, low, close]` in a single tuple to minimize `request.security()` call limits.

---

## Plotting Bars with `plotbar()`

The signature of `plotbar()` is:
```pinescript
plotbar(open, high, low, close, title, color, editable, show_last, display, force_overlay) → void
```

Conventional bars do not have wicks or borders. Consequently, `plotbar()` lacks parameters for `bordercolor` and `wickcolor`.

### Dual-Color Conventional Bars
This script plots conventional bars using conditional color inputs:
```pinescript
//@version=6
indicator("Dual-color bars")
paletteColor = close >= open ? color.lime : color.red
plotbar(open, high, low, close, color = paletteColor)
```
