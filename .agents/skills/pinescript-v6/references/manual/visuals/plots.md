# Plots

The `plot()` function is the most frequently used built-in for displaying calculated time-series information. It is highly versatile and supports rendering lines, histograms, filled areas, columns, circles, or crosses.

---

## Simple Plot Configurations

This script showcases multiple styles of standard plots overlayed on a price chart:

```pinescript
//@version=6
indicator("`plot()`", "", true)

// Dashed line (default 1-pixel width)
plot(high, "Dashed blue `high` line", linestyle = plot.linestyle_dashed)

// Large lime/purple crosses at body centers
plot(math.avg(close, open), "Crosses in body center", close > open ? color.lime : color.purple, 6, plot.style_cross)

// Thick step-line at body lows
plot(math.min(open, close), "Navy step line on body low point", color.navy, 3, plot.style_stepline)

// Simple gray circles at bar lows
plot(low, "Gray dot on `low`", color.gray, 3, plot.style_circles)

// Smooth color-state transition (looking back 2 bars)
color VIOLET = #AA00FF
color GOLD   = #CCCC00
ma = ta.alma(hl2, 40, 0.85, 6)
var almaColor = color.silver
almaColor := ma > ma[2] ? GOLD : ma < ma[2] ? VIOLET : almaColor
plot(ma, "Two-color ALMA", almaColor, 2)
```

---

## Plotting Volumes and Histograms (Separate Pane)

This script plots volume stats in a separate pane:

```pinescript
//@version=6
indicator("Volume change", format = format.volume)

color GREEN         = #008000
color GREEN_LIGHT   = color.new(GREEN, 50)
color GREEN_LIGHTER = color.new(GREEN, 85)
color PINK          = #FF0080
color PINK_LIGHT    = color.new(PINK, 50)
color PINK_LIGHTER  = color.new(PINK, 90)

bool  barUp = ta.rising(close, 1)
bool  barDn = ta.falling(close, 1)
float volumeChange = ta.change(volume)

// Wide columns for volume
volumeColor = barUp ? GREEN_LIGHTER : barDn ? PINK_LIGHTER : color.gray
plot(volume, "Volume columns", volumeColor, style = plot.style_columns)

// Histograms for volume change (width controlled by linewidth)
volumeChangeColor = barUp ? volumeChange > 0 ? GREEN : GREEN_LIGHT : volumeChange > 0 ? PINK : PINK_LIGHT
plot(volumeChange, "Volume change columns", volumeChangeColor, 12, plot.style_histogram)

// Reference zero level
plot(0, "Zero line", color.gray)
```

### Key Execution Rules:
*   **Global Scope Constraint**: A `plot()` call must **always** be placed in the script's global scope (non-indented lines). You cannot call it inside local scopes (functions, conditional blocks, loops).
*   **Force Overlay**: Setting `force_overlay = true` forces the plot to display on the main chart pane even if the indicator itself executes in a separate sub-pane.

---

## `plot()` Parameters

The signature of `plot()` is:
```pinescript
plot(series, title, color, linewidth, style, trackprice, histbase, offset, join, editable, show_last, display, format, precision, force_overlay, linestyle) → plot
```

### Plotted Styles (`style` parameter)
*   `plot.style_line` (Default): Continuous line bridging over `na` gaps.
*   `plot.style_linebr`: Discontinuous line leaving gaps on `na` values.
*   `plot.style_stepline`: Staircase transitions.
*   `plot.style_area` / `plot.style_areabr`: Filled gradient/translucent color between the line and the `histbase` level.
*   `plot.style_columns`: Vertical bars (like volume columns). Linewidth has no effect.
*   `plot.style_histogram`: Thin vertical bars where the pixel width is specified by `linewidth`.
*   `plot.style_circles` / `plot.style_cross`: Discrete marks at each bar. Can be linked by a 1-pixel joining thread if `join = true`.

### Visibility Locations (`display` parameter)
The default is `display.all` (displays on pane, status line, price scale, and Data Window). You can combine options using addition and subtraction:
```pinescript
// Hide pane rendering but show values in Data Window and Status Line
plot(close, "Data Only", display = display.all - display.pane)
```

Setting `display = display.none` calculates values invisibly without affecting chart scale. This is ideal for plots intended solely for alert placeholders:
```pinescript
plot(r, "RSI", display = display.none)
alertcondition(xUp, "xUp alert", message = 'RSI is bullish at: {{plot("RSI")}}')
```

---

## Plotting Conditionally

While `plot()` cannot reside inside `if` blocks, you can calculate conditional price levels or colors globally:

### 1. Conditional Price Series
Pass `na` as the plot value when conditions are not met:
```pinescript
//@version=6
indicator("Discontinuous plots", "", true)
bool plotValues = bar_index % 3 == 0

// Discontinuous line
plot(plotValues ? high : na, color = color.fuchsia, linewidth = 6, style = plot.style_linebr)

// Discontinuous markers
plot(plotValues ? math.max(open, close) : na, color = color.navy, linewidth = 6, style = plot.style_circles)
```

### 2. Eliminating Transition Lines (Pivots)
When tracking levels that shift periodically, set the plot color to `na` during transition bars to avoid drawing diagonal slopes:
```pinescript
//@version=6
indicator("Pivot plots", "", true)

// Fill gaps using fixnan
pivotHigh = ta.valuewhen(not na(ta.pivothigh(3, 3)), ta.pivothigh(3, 3), 0)

// Set color to na on the transition bar to hide the line join
plot(pivotHigh, "High pivot", ta.change(pivotHigh) != 0 ? na : color.olive, 3)
```

---

## Levels and Offsets

### Offsets
The `offset` parameter shifts the plotted series forward (positive values) or backward (negative values) by a fixed number of bars:
```pinescript
//@version=6
indicator("Offset demo", overlay = true)
plot(close, "Offset in past", color = color.red, offset = -5)
plot(close, "Offset in future", color = color.lime, offset = 5)
```

### Levels (Simulating hline)
When you need to draw horizontal benchmark lines at dynamic coordinates (which `hline` does not support since its price parameters require constant inputs), use `plot()`:
```pinescript
//@version=6
indicator("CCI levels with `plot()`")
plot(ta.cci(close, 20))

// Simulating hline using trackprice to draw a benchmark across the full screen width
plot(200, "200 Level", color.green, 2, trackprice = true, show_last = 1, offset = -99999)
```

---

## Plot Limits and Scaling

### Limit Counts
Each script can display a maximum of **64 plots** (including `plot*()` functions and `alertcondition()`).
*   A plot with a constant color (`const color` or hex code) counts as **one plot**.
*   A plot using a dynamic color (`simple color` or `series color`) counts as **two plots** toward the limit.

### Scale Distortions and Merging
If indicators with radically different numeric bounds (e.g. BTCUSD price vs. RSI 0-100) are plotted in the same pane, the price scale becomes heavily distorted.

To merge two indicators with different ranges without scale distortion, compress and shift one of the indicator outputs:
```pinescript
//@version=6
indicator("RSI and TSI")

myRSI = ta.rsi(close, 20)
plot(myRSI, "RSI", color.lime, 3)

// Compress TSI (normally -100/100) by dividing by 2, and shift it up by 150
myTSI = 150 + (100 * ta.tsi(close, 13, 25) / 2)

plot(myTSI, "Shifted TSI", color.blue, 2)
hline(150, "TSI Zero Level", color = color.gray)
```
