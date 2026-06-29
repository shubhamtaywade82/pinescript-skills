# Tables

Tables are special drawing objects used to position structured grids of information in specific, fixed screen locations. Contrary to all other drawings in Pine Script®, tables are not anchored to chart bars or price coordinates. Instead, they float in the script's visual pane, maintaining their screen position and scale regardless of chart zooming, panning, or cursor movement.

Tables are constructed and populated in two distinct steps:
1.  **Structure Definition**: Call `table.new()` to initialize columns, rows, background color, border styles, and layout anchor. This return value is a `table` ID (pointer).
2.  **Cell Definition**: Call `table.cell()` separately to populate cell contents (strings, sizes, alignments, and background fills).

Most attributes of tables and individual cells can be modified dynamically during runtime using setter functions (e.g. `table.set_bgcolor()` or `table.cell_set_text_color()`).

---

## Layout Anchors and Sizing

A table is positioned in the visual space by anchoring it to one of nine references:
*   `position.top_left`, `position.top_center`, `position.top_right`
*   `position.middle_left`, `position.middle_center`, `position.middle_right`
*   `position.bottom_left`, `position.bottom_center`, `position.bottom_right`

### Sizing Modes
*   **Automatic Mode (Default)**: Cell widths and heights automatically adjust to accommodate the longest string in each row or column.
*   **Explicit Mode**: Width and height are defined as a percentage of the available visual pane space (from 0 to 100).

---

## Execution Optimization Guidelines

Displayed table contents always reflect the final state computed on the **last historical bar** or the **latest realtime tick**. Unlike status lines or plots, variables printed inside table cells **do not update** as users hover their cursors over historical bars. 

To prevent redundant iterations and save server resources:
*   **Always declare** table objects using the `var` keyword so they initialize only once.
*   **Enclose cell updates** inside `if barstate.islast` blocks so they only execute on the final bar.
*   *Note*: Ensure indicators and calculations (like moving averages) are evaluated **globally** before the conditional block, as functions called inside an `if barstate.islast` block will fail to build their historical buffers properly.

---

## Simple Value Overlay Example (ATR)

This script plots the Average True Range (ATR) in the top-right corner of the chart:

```pinescript
//@version=6
indicator("ATR", "", true)

atrPeriodInput = input.int(14, "ATR period", minval = 1, tooltip = "Using a period of 1 yields True Range.")

// Initialize the table structure once on bar zero
var table atrDisplay = table.new(position.top_right, 1, 1, bgcolor = color.gray, frame_width = 2, frame_color = color.black)

// Calculate the ATR value globally on every bar
myAtr = ta.atr(atrPeriodInput)

// Populate the table cell ONLY on the last bar
if barstate.islast
    table.cell(atrDisplay, 0, 0, str.tostring(myAtr, format.mintick), text_color = color.white)
```

---

## Coloring the Chart Pane Background

This example uses a single full-screen table cell to color the chart background green or red based on the current RSI value:

```pinescript
//@version=6
indicator("Chart background", "", true)

bullColorInput = input.color(color.new(color.green, 95), "Bull", inline = "1")
bearColorInput = input.color(color.new(color.red, 95), "Bear", inline = "1")

colorChartBg(bullColor, bearColor) =>
    var table bgTable = table.new(position.middle_center, 1, 1)
    float r = ta.rsi(close, 20)
    color bgColor = r > 50 ? bullColor : r < 50 ? bearColor : na
    
    // Execute full screen cell covering only on the last bar
    if barstate.islast
        table.cell(bgTable, 0, 0, width = 100, height = 100, bgcolor = bgColor)

colorChartBg(bullColorInput, bearColorInput)
```

---

## Sophisticated Display Panels (Price vs. Moving Averages)

This script generates a dashboard panel showing user-configured moving averages. The cells highlight green or red depending on whether the price is trading above or below each average:

```pinescript
//@version=6
indicator("Price vs MA", "", true)

var string GP1 = "Moving averages"
int masQtyInput    = input.int(20, "Quantity", minval = 1, maxval = 40, group = GP1, tooltip = "1-40")
int masStartInput  = input.int(20, "Periods begin at", minval = 2, maxval = 200, group = GP1, tooltip = "2-200")
int masStepInput   = input.int(20, "Periods increase by", minval = 1, maxval = 100, group = GP1, tooltip = "1-100")

var string GP2 = "Display"
string tableYposInput = input.string("top", "Panel position", inline = "11", options = ["top", "middle", "bottom"], group = GP2)
string tableXposInput = input.string("right", "", inline = "11", options = ["left", "center", "right"], group = GP2)
color  bullColorInput = input.color(color.new(color.green, 30), "Bull", inline = "12", group = GP2)
color  bearColorInput = input.color(color.new(color.red, 30), "Bear", inline = "12", group = GP2)
color  neutColorInput = input.color(color.new(color.gray, 30), "Neutral", inline = "12", group = GP2)

// Map user inputs to position flags
string tablePosition = switch
    tableXposInput == "left"   and tableYposInput == "top"    => position.top_left
    tableXposInput == "left"   and tableYposInput == "middle" => position.middle_left
    tableXposInput == "left"   and tableYposInput == "bottom" => position.bottom_left
    tableXposInput == "center" and tableYposInput == "top"    => position.top_center
    tableXposInput == "center" and tableYposInput == "middle" => position.middle_center
    tableXposInput == "center" and tableYposInput == "bottom" => position.bottom_center
    tableXposInput == "right"  and tableYposInput == "top"    => position.top_right
    tableXposInput == "right"  and tableYposInput == "middle" => position.middle_right
    tableXposInput == "right"  and tableYposInput == "bottom" => position.bottom_right
    => position.top_right

var table panel = table.new(tablePosition, 2, masQtyInput + 1)

if barstate.islast
    table.cell(panel, 0, 0, "MA", bgcolor = neutColorInput)
    table.cell(panel, 1, 0, "Value", bgcolor = neutColorInput)

int period = masStartInput
for i = 1 to masQtyInput
    // Calculate the moving average globally on every bar
    float ma = ta.sma(close, period)
    
    // Only modify table cells on the last bar
    if barstate.islast
        table.cell(panel, 0, i, str.tostring(period), bgcolor = neutColorInput)
        color bgColor = close > ma ? (open < ma ? neutColorInput : bullColorInput) : (open > ma ? neutColorInput : bearColorInput)
        table.cell(panel, 1, i, str.tostring(ma, format.mintick), text_color = color.black, bgcolor = bgColor)
    period += masStepInput
```

---

## Visualizing a Heatmap

In this script, a table is positioned at the bottom of the chart to display a multi-period price proximity heatmap:

```pinescript
//@version=6
indicator("Price vs Past", "", true)

var int MAX_LOOKBACK = 300

int   lookBackInput  = input.int(150, minval = 1, maxval = MAX_LOOKBACK, step = 10)
color bullColorInput = input.color(#00FF00ff, "Bull", inline = "11")
color bearColorInput = input.color(#FF0080ff, "Bear", inline = "11")

drawHeatmap(src, lookBack) =>
    // Enforce history buffer for lookback calculations
    max_bars_back(src, MAX_LOOKBACK)
    
    if barstate.islast
        var heatmap = table.new(position.bottom_center, lookBack, 1)
        for i = 1 to lookBack
            float transp = 100.0 * i / lookBack
            if src > src[i]
                table.cell(heatmap, lookBack - i, 0, bgcolor = color.new(bullColorInput, transp))
            else
                table.cell(heatmap, lookBack - i, 0, bgcolor = color.new(bearColorInput, transp))

drawHeatmap(high, lookBackInput)
```

---

## Key Implementation Tips

*   **Strategy Constraints**: In strategy scripts, table updates wrapped inside `if barstate.islast` blocks will only execute during historical bars and on completed realtime candles unless you enable `calc_on_every_tick = true` in the strategy declaration.
*   **Property Overwriting**: Each call to `table.cell()` completely redefines that cell's properties, resetting any parameter omitted from the call. If you need to edit a single property (e.g. text only), use specific setters like `table.cell_set_text()` instead.
*   **Resource Management**: Keep cell counts reasonable. Very large grids (especially those recalculating lookbacks dynamically) can increase script execution time. Always restrict cell rendering loops to `barstate.islast`.
