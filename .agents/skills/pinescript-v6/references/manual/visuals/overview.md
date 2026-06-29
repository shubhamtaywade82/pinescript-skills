# Visuals Overview

Well-designed visuals make indicators and strategies easier to use and less cluttered. Each visual element presents data differently:
*   **Plot visuals** include all `plot*()` functions, horizontal levels, background and bar coloring, and fills.
*   **Drawing visuals** include lines, polylines, linefills, boxes, labels, and tables.

Scripts can configure where and how visual elements appear by using script-wide settings.

> [!NOTE]
> Users can draw directly on charts using standard chart Drawing Tools. Pine scripts cannot interact with these manual drawings, and cursor actions on the chart do not modify Pine drawing objects.

---

## Script-Wide Visual Settings

These settings control how all of the script’s outputs collectively appear on the chart, configured as parameters in the `indicator()` or `strategy()` declaration statement.

### overlay
Controls whether the script’s outputs appear in the main chart pane or a separate pane.
*   By default (`overlay = false`), visual outputs display in a separate pane.
*   Individual elements can override this layout by specifying `force_overlay = true` inside their respective functions (e.g. `plot()`, `bgcolor()`, `label.new()`), placing that specific visual directly on the main chart.

### scale
Specifies the y-axis scale that the pane visuals use.
*   `scale.none` uses the main chart's price scale (default for overlayed indicators).
*   `scale.right` or `scale.left` generates a new scale distinct from the main chart's price scale.

### behind_chart
Specifies whether visuals appear behind or in front of the main chart series bars.
*   `behind_chart = true` (default) places visuals behind candles.
*   `behind_chart = false` displays them in front, which can obscure price bars depending on opacity.

> [!IMPORTANT]
> Visual settings in the script declaration statement are evaluated **only once** when the script first loads. Modifying parameters like `overlay` in the code of an active script on the chart will not update its current display until you remove it and add a new instance.

---

## Plot Visuals

The outputs of the following functions are classified as plot visuals:
*   **Plot functions**: `plot()`, `plotshape()`, `plotchar()`, `plotarrow()`, `plotbar()`, `plotcandle()`
*   **Coloring**: `barcolor()`, `bgcolor()`
*   **Levels**: `hline()`
*   **Fills**: `fill()`

Plots are **serialized visuals** that return a value on every single chart bar (even if that value is `na`). 

### Display Locations
Plots can display numerical outputs in:
1.  The chart pane.
2.  The price scale (last value).
3.  The script's status line.
4.  The Data Window.

Using the `display` parameter, plots can target specific locations (supporting addition and subtraction operations for combining configurations):

```pinescript
//@version=6
indicator("Plot visuals display demo", overlay = false)

float barCO = close - open

// Horizontal lines and fills support binary visibility states
h1 = hline(125, "Level 125", linewidth = 2, display = display.none)
h2 = hline(100, "Level 100", linewidth = 2, display = display.all)
fill(h1, h2, color = color.new(color.blue, 90), display = display.all)

// plot*() functions allow granular display combinations
plot(close, "Close", color.blue, 3, display = display.all)
plot(open,  "Open",  color.orange, 3, display = display.all - display.pane)
plotarrow(barCO, "Bar CO", color.green, color.red, display = display.status_line + display.data_window)
plotshape(barCO > 5, "Large CO", shape.circle, location.abovebar, color.fuchsia, display = display.pane)
```

### External Uses
Unlike drawing objects, plot outputs are used for:
*   **Data Exporting**: Plot results are included when users export chart data to a CSV file.
*   **UI Alerts**: Users can configure alert conditions directly using visual plots from the chart UI.
*   **Source Input**: Scripts can pull in plot values from other indicators on the chart as input variables via `input.source()`.
*   **Pine Screener**: The Pine Screener scans watchlists and filters symbols based on active indicator plot values.

---

## Drawing Visuals

The following elements are classified as drawing visuals:
*   Lines
*   Polylines
*   Linefills
*   Boxes
*   Labels
*   Tables

Drawings are **instantiated objects** rather than series data. They do not automatically evaluate on every bar, but exist as unique entities at coordinates you specify.

### Customization via User Inputs
Since drawings accept `series` variables, you can make drawings customizable from the "Inputs" tab:

```pinescript
//@version=6
indicator("Customizable drawings demo", overlay = true)

const string G1 = "Table Style"
const string G2 = "Label Style"

string tbVerticalInput   = input.string("Top", "Position", ["Top", "Middle", "Bottom"], inline = "Pos", group = G1)
string tbHorizontalInput = input.string("Right", "Center", ["Left", "Center", "Right"], inline = "Pos", group = G1)
color  tbBackgroundInput = input.color(#ffeb3bb3, "Background color", inline = "Col", group = G1)
color  tbBorderInput     = input.color(color.white,"Border color",    inline = "Col", group = G1)
string tbTextSizeInput   = input.string(size.large, "Text size", inline = "Txt", group = G1,
     options = [size.tiny, size.small, size.normal, size.large, size.huge, size.auto])
color tbTextColorInput   = input.color(color.black, "Text color", inline = "Txt", group = G1)

int   lblSizeInput      = input.int(16, "Label size", minval = 0, inline = "Lbl", group = G2)
color lblColorInput     = input.color(color.orange, "Label color", inline = "Lbl", group = G2)
color lblTextColorInput = input.color(color.white, "Text color", group = G2)

if barstate.islastconfirmedhistory
    string tbPos = switch
        tbVerticalInput == "Top"    and tbHorizontalInput == "Left"   => position.top_left
        tbVerticalInput == "Top"    and tbHorizontalInput == "Center" => position.top_center
        tbVerticalInput == "Top"    and tbHorizontalInput == "Right"  => position.top_right
        tbVerticalInput == "Middle" and tbHorizontalInput == "Left"   => position.middle_left
        tbVerticalInput == "Middle" and tbHorizontalInput == "Center" => position.middle_center
        tbVerticalInput == "Middle" and tbHorizontalInput == "Right"  => position.middle_right
        tbVerticalInput == "Bottom" and tbHorizontalInput == "Left"   => position.bottom_left
        tbVerticalInput == "Bottom" and tbHorizontalInput == "Center" => position.bottom_center
        tbVerticalInput == "Bottom" and tbHorizontalInput == "Right"  => position.bottom_right
        => position.top_right
        
    var table displayTable = table.new(tbPos, 2, 2, tbBackgroundInput, border_color = tbBorderInput, border_width = 1) 
    displayTable.cell(0, 0, "Open",              text_color = tbTextColorInput, text_size = tbTextSizeInput)
    displayTable.cell(1, 0, str.tostring(open),  text_color = tbTextColorInput, text_size = tbTextSizeInput)
    displayTable.cell(0, 1, "Close",             text_color = tbTextColorInput, text_size = tbTextSizeInput)
    displayTable.cell(1, 1, str.tostring(close), text_color = tbTextColorInput, text_size = tbTextSizeInput)

    string lblText = "Bar body = " + str.tostring(close - open)
    label.new(bar_index, high, lblText, color = lblColorInput, textcolor = lblTextColorInput, size = lblSizeInput)
```

---

## Z-Index Ordering

The z-index determines which visuals appear stacked on top of others. Elements are grouped into regions of z-space listed here in **ascending order** (elements at the bottom of the list render on top of elements above them):

1.  Background colors (`bgcolor`)
2.  Fills (`fill`)
3.  Plots (`plot*`)
4.  Horizontal levels (`hline`)
5.  Linefills (`linefill`)
6.  Lines (`line`)
7.  Boxes (`box`)
8.  Labels (`label`)
9.  Tables (`table`)

Plots, horizontal levels, and fills can have their z-order sorted in the order they appear in the code by declaring `explicit_plot_zorder = true` in the script header.

---

## Summary of Visual Elements

### `plot()`
Plots a continuous numeric data series. Supports styles such as lines, steps, histograms, areas, crosses, and circles. Ideal for overlays like moving averages or indicators like MACD.

### `plotshape()` and `plotchar()`
Plot symbols, shapes, or alphanumeric Unicode characters at absolute prices or relative offsets (e.g. `location.abovebar`, `location.bottom`). Useful for marking buy/sell signals or debugging conditions without distorting the price scale.

### `plotarrow()`
Plots upward/downward arrows with lengths adjusted relative to the values of a series. Useful for displaying price gaps or divergence strength.

### `plotbar()` and `plotcandle()`
Plots custom bar sets or candles on the chart. Every call defines 4 points (O, H, L, C) and consumes 4 plot slots.

### `hline()`
Plots a static horizontal boundary line extending across the entire chart. Faster and more resource-efficient than a series-based plot line.

### `bgcolor()` and `barcolor()`
Color the background area of the pane behind a bar (`bgcolor`) or change the body color of candles (`barcolor`).

### `fill()`
Colors the pane region between two plots or two `hline` levels.

### Lines and Polylines
*   **Lines** connect two points. Can be dynamically updated using setter methods.
*   **Polylines** connect up to 10,000 coordinates sequentially. Closed polylines can fill their inner boundaries.
```pinescript
//@version=6
indicator("Polyline drawing demo", overlay = true)

int pivotLegsInput = input.int(5, "Pivot leg length", minval = 1)
bool isCurvedPolyline = input.bool(false, "Use curved polyline")

var array<chart.point> pointsArray = array.new<chart.point>()
float pivotHigh = ta.pivothigh(pivotLegsInput, pivotLegsInput)
float pivotLow = ta.pivotlow(pivotLegsInput, pivotLegsInput)

if not na(pivotHigh)
    chart.point highPoint = chart.point.from_index(bar_index - pivotLegsInput, pivotHigh)
    pointsArray.push(highPoint)
    label.new(highPoint, "Pivot: " + str.tostring(pivotHigh, "##.##"))
if not na(pivotLow)
    chart.point lowPoint = chart.point.from_index(bar_index - pivotLegsInput, pivotLow)
    pointsArray.push(lowPoint)
    label.new(lowPoint, "Pivot: " + str.tostring(pivotLow, "##.##"), style = label.style_label_up)

if barstate.islastconfirmedhistory
    for i = (pointsArray.size() - 1) to 0
        chart.point point = pointsArray.get(i)
        if (bar_index - point.index) > 9999
            pointsArray.remove(i)

    polyline.new(pointsArray, curved = isCurvedPolyline, line_color = color.purple, line_width = 4)
```

### Linefills
Fill the region of space between two `line` objects.

### Boxes
Draw custom rectangles on the chart using coordinate pairs. Can display text, auto-wrap strings, and adjust border styles.

### Labels
Display dynamic text values anywhere on the chart. Support dynamic string variables and customize positioning shapes (e.g. pointer bubbles, callouts, arrows).

### Tables
Display grids of information (like dashboards or symbol statistics) fixed to one of nine screen coordinates, independent of chart zooming or scrolling.
```pinescript
//@version=6
indicator("Timeframe warning table demo", overlay = true, max_labels_count = 500, behind_chart = false) 

sessionInput = input.session("0930-1600", "Trading Session")

if barstate.islastconfirmedhistory and timeframe.isdwm
    var table warningTable = table.new(position.middle_center, 1, 1, color.yellow)
    warningTable.cell(0, 0, 
         "Warning: This indicator supports only intraday timeframes.\n Switch to a lower timeframe to see output.", 
         text_size = size.large)

if timeframe.isintraday and na(time("", sessionInput)[1]) and not na(time("", sessionInput))
    label.new(bar_index, open, "Session Open: " + str.tostring(open, "#.##"), yloc = yloc.abovebar, 
      color = color.green, style = label.style_label_down)

if timeframe.isintraday and not na(time("", sessionInput)[1]) and na(time("", sessionInput))
    label.new(bar_index[1], close[1], "Session Close: " + str.tostring(close[1], "#.##"), yloc = yloc.belowbar, 
      color = color.red, style = label.style_label_up)
```
