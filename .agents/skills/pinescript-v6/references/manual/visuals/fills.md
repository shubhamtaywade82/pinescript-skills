# Fills

Fills allow you to shade the chart space occupied by your indicator plots, levels, lines, boxes, and polylines. Three separate mechanisms facilitate fills in Pine Script®:

1.  **`fill()` function**: Fills the space between two `plot()` or `hline()` IDs.
2.  **`linefill` drawings**: Objects that fill the space between two `line.new()` instances.
3.  **Drawing-specific properties**: Translucent properties inside drawing constructors, such as `bgcolor` in `box.new()` or `fill_color` in `polyline.new()`.

---

## Plot and `hline` Fills

The `fill()` function supports two signatures:
```pinescript
fill(plot1, plot2, color, title, editable, show_last, fillgaps) → void
fill(hline1, hline2, color, title, editable, fillgaps) → void
```

> [!IMPORTANT]
> The `fill()` function requires either **two plot IDs** or **two hline IDs**. You cannot mix plot and hline arguments in the same call. If you need to shade the space between a fixed price level and a fluctuating line, use a zero or level `plot()` instead of an `hline()`.

### 1. Basic Plot and Level Fills
```pinescript
//@version=6
indicator("Example 1")

// Assign "plot" IDs
p1 = plot(math.sin(high), "Sine of `high`")
p2 = plot(math.cos(low), "Cosine of `low`")
p3 = plot(math.sin(close), "Sine of `close`")

// Fill between plot IDs
fill(p1, p3, color.new(color.red, 90), "`p1`-`p3` fill")
fill(p2, p3, color.new(color.blue, 90), "`p2`-`p3` fill")

// Assign "hline" IDs
h1 = hline(0, "First level")
h2 = hline(1.0, "Second level")
h3 = hline(0.5, "Third level")
h4 = hline(1.5, "Fourth level")

// Fill between hline IDs
fill(h1, h2, color.new(color.yellow, 90), "`h1`-`h2` fill")
fill(h3, h4, color.new(color.lime, 90), "`h3`-`h4` fill")
```

### 2. Plotting an hline Alternative for Fills
Since you cannot mix types, zero lines must sometimes be plotted as regular lines to support shading:
```pinescript
//@version=6
indicator("Example 2")

float ma = ta.sma(close, 10)
float oscillator = 100 * (ma - close) / ma

oscPlotID = plot(oscillator, "Oscillator")

// Plot the zero line using plot() instead of hline() to support fill()
zeroPlotID = plot(0, "Zero level", color.silver, 1, plot.style_circles)

fill(oscPlotID, zeroPlotID, color.new(color.blue, 90), "Oscillator fill")
```

### 3. Conditional Series Color Fills (Moving Average Ribbon)
The color parameter inside `fill()` accepts "series color" values, enabling dynamic fills:
```pinescript
//@version=6
indicator("Example 3", overlay = true)

float ma1 = ta.sma(close, 5)
float ma2 = ta.sma(close, 20)

// Conditional fill color
color fillColor = ma1 > ma2 ? color.new(color.green, 90) : color.new(color.red, 90) 

ma1PlotID = plot(ma1, "5-bar SMA")
ma2PlotID = plot(ma2, "20-bar SMA")

fill(ma1PlotID, ma2PlotID, fillColor, "SMA plot fill")
```

---

## Line Fills (`linefill` Objects)

The `fill()` function cannot reference `line` drawing objects. To fill the space between two lines, construct a `linefill` object via `linefill.new()`:

```pinescript
linefill.new(line1, line2, color) → series linefill
```

### Dynamic Channel Fills Example
This script traces pivot highs and lows using extended lines on the last confirmed historical bar and fills the space between them:

```pinescript
//@version=6
indicator("Linefill demo", "Channel", true)

int LEFT_BARS  = 15
int RIGHT_BARS = 5

float pivotHigh = ta.pivothigh(LEFT_BARS, RIGHT_BARS)
float pivotLow  = ta.pivotlow(LEFT_BARS, RIGHT_BARS)

var firstHighPoint  = chart.point.new(na, na, na)
var secondHighPoint = chart.point.new(na, na, na)
var firstLowPoint   = chart.point.new(na, na, na)
var secondLowPoint  = chart.point.new(na, na, na)

if not na(pivotHigh)
    firstHighPoint  := secondHighPoint
    secondHighPoint := chart.point.from_index(bar_index - RIGHT_BARS, pivotHigh)
if not na(pivotLow)
    firstLowPoint  := secondLowPoint
    secondLowPoint := chart.point.from_index(bar_index - RIGHT_BARS, pivotLow)

if barstate.islastconfirmedhistory
    line pivotHighLine = line.new(firstHighPoint, secondHighPoint, extend = extend.right)
    line pivotLowLine  = line.new(firstLowPoint, secondLowPoint, extend = extend.right)
    
    color fillColor = switch
        secondHighPoint.price > firstHighPoint.price and secondLowPoint.price > firstLowPoint.price => color.lime
        secondHighPoint.price < firstHighPoint.price and secondLowPoint.price < firstLowPoint.price => color.red
        => color.silver
        
    linefill.new(pivotHighLine, pivotLowLine, color.new(fillColor, 90))
```

*   **Line limits**: Any two lines can only have one active `linefill` between them. Consecutive calls using the same lines replace the existing fill.
*   **Properties**: You can retrieve referenced lines from a linefill using `linefill.get_line1()` and `linefill.get_line2()`.

---

## Box and Polyline Fills

For drawings like boxes and polylines, fill colors are configured inside their constructor calls using the `bgcolor` or `fill_color` parameters.

### Inscribed Shapes Example
This script draws a polyline octagon and a box rectangle on the last confirmed historical bar, using custom transparency fields to fill their inner areas:

```pinescript
//@version=6
indicator("Box and polyline fills demo")

var int barCount = 0
if time > chart.left_visible_bar_time and time < chart.right_visible_bar_time
    barCount += 1

int radius = math.ceil(barCount / 4)

if barstate.islastconfirmedhistory
    array<chart.point> points = array.new<chart.point>()
    float angle = 0.0
    float increment = 0.25 * math.pi
    
    for i = 0 to 7
        int x = int(math.round(math.cos(angle) * radius))
        float y = math.round(math.sin(angle) * radius)
        points.push(chart.point.from_index(bar_index - radius + x, y))
        angle += increment
        
    // Draw octagon and fill it with translucent blue
    polyline.new(points, closed = true, fill_color = color.new(color.blue, 60))
    
    // Draw inscribed rectangle and fill it with opaque purple
    box.new(points.get(3), points.get(7), bgcolor = color.purple)
```
