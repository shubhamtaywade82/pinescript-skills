# Debugging

TradingView’s close integration between the Pine Editor and the Supercharts interface enables efficient, interactive debugging of Pine Script® code. Pine scripts can create dynamic outputs in multiple locations, on and off the chart. Programmers can use these outputs to validate their scripts’ behaviors and ensure everything works as expected.

Understanding the most effective tools and methods for inspecting a script helps programmers quickly find and fix potential problems in their code, which improves the overall coding experience. This page explains the script outputs that are the most useful for debugging, along with helpful tips and techniques.

> [!TIP]
> Effective debugging in the Pine Script environment requires an understanding of the Execution model, Time series structure, and Type system. We recommend reviewing these topics, along with string formatting, which the following techniques often use.

---

## Common Debug Outputs

Pine scripts can create outputs in several ways, each of which has different advantages. While programmers can use any of them to debug their code, some outputs are more optimal for debugging than others.

### 1. Pine Logs (`log.*` namespace)
The functions in the `log.*` namespace log interactive messages in the Pine Logs pane. These logging functions are the most convenient and flexible tools for debugging Pine code. Scripts can call `log.*()` functions on any execution from global or local scopes, enabling programmers to analyze historical and realtime script behaviors in depth with minimal code.

```pinescript
//@version=6
indicator("Common debug outputs - Pine Logs")

//@variable The natural logarithm of the current `high - low` range.
float logRange = math.log(high - low)

// Plot the `logRange`.
plot(logRange, "logRange")

if barstate.isconfirmed
    // Generate an "error" or "info" message on the confirmed bar, depending on whether `logRange` is defined.
    switch 
        na(logRange) => log.error("Undefined `logRange` value.")
        =>              log.info("`logRange` value: " + str.tostring(logRange))
else
    // Generate a "warning" message for unconfirmed values.
    log.warning("Unconfirmed `logRange` value: " + str.tostring(logRange))
```

### 2. Pine Drawings
Pine drawings display visuals in the main chart pane or the script’s separate pane. Drawings provide convenient ways to visualize a script’s data and logic within global or local scopes. Labels are the most flexible drawings for debugging, because they can display colored shapes with formatted text and tooltips at any available chart location.

```pinescript
//@version=6
indicator("Common debug outputs - Pine drawings", overlay = true)

//@variable Is `true` when a new daily bar opens.
bool newDailyBar = timeframe.change("1D")
//@variable The previous bar's `bar_index` from when `newDailyBar` last occurred.
int closedIndex = ta.valuewhen(newDailyBar, bar_index - 1, 0)
//@variable The previous bar's `close` from when `newDailyBar` last occurred.
float closedPrice = ta.valuewhen(newDailyBar, close[1], 0)

if newDailyBar
    // Draw a line from the previous `closedIndex` and `closedPrice` to current values.
    line.new(closedIndex[1], closedPrice[1], closedIndex, closedPrice, width = 2)
    //@variable A string containing debug information to display in a label.
    string debugText = "'1D' bar closed at: \n(" + str.tostring(closedIndex) + ", " + str.tostring(closedPrice) + ")"
    //@variable Draws a label at the current `closedIndex` and `closedPrice`.
    label debugLabel = label.new(closedIndex, closedPrice, debugText, color = color.purple, textcolor = color.white)
```

### 3. Plots (`plot*()` functions)
The `plot*()` functions can help to debug numeric values, conditions, and colors from a script’s global scope. They can output results in up to four locations: the main chart pane or the script’s pane, the status line, the price scale, and the Data Window.

```pinescript
//@version=6
indicator("Common debug outputs - Plots")

// Plot the `bar_index` in all available locations.
plot(bar_index, "bar_index", color = color.teal, linewidth = 3)
```

### 4. Background and Bar Colors (`bgcolor()` / `barcolor()`)
The `bgcolor()` function displays colors in the background of the main chart pane or the script’s pane. The `barcolor()` function colors the main chart’s bars or candles.

```pinescript
//@version=6
indicator("Common debug outputs - Background and bar colors")

//@variable Is `true` if the `close` is rising over 2 bars.
bool risingPrice = ta.rising(close, 2)

// Highlight the chart background and color the main chart bars based on `risingPrice`.
bgcolor(risingPrice ? color.new(color.green, 70) : na, title= "`risingPrice` highlight")
barcolor(risingPrice ? color.aqua : chart.bg_color, title = "`risingPrice` bar color")
```

---

## Pine Logs

Pine Logs are interactive, user-defined messages that scripts can create from within global or local scopes at any point during code executions on the chart’s dataset or requested datasets. They provide a simple, powerful way for programmers to inspect a script’s calculations, logic, and execution flow with human-readable text. Using Pine Logs is the primary, most universal technique for debugging Pine Script code.

Pine Logs do not appear on the chart or in the Data Window. Instead, scripts print logged messages with prefixed date and time information in the dedicated **Pine Logs** pane. 

To access the pane, select "Pine Logs" from the Pine Editor's "More" menu or from the status line of a running script.

> [!IMPORTANT]
> **Notice**: Only personal scripts can generate Pine Logs. A published script cannot create logs, even if its source code contains `log.*()` function calls. Published libraries can export functions containing `log.*()` calls for use in personal scripts, but they cannot generate logs directly.

### Creating Logs
Scripts create Pine Logs by calling the functions in the `log` namespace: `log.info()`, `log.warning()`, or `log.error()`. They have the following two signatures:
*   `log.*(message) → void`
*   `log.*(formatString, arg0, arg1, ...) → void`

Each function maps to a different level in the Pine Logs pane:
*   `log.info()`: Info level (gray text).
*   `log.warning()`: Warning level (orange text).
*   `log.error()`: Error level (red text).

```pinescript
//@version=6
indicator("Logging levels demo", overlay = true)

// Display logs with all three logging levels in the Pine Logs pane on the first bar. 
if barstate.isfirst
    log.info("This is an 'info' message.")
    log.warning("This is a 'warning' message.")
    log.error("This is an 'error' message.")
```

During historical executions, scripts log a message once per call per bar. During realtime executions, scripts log messages for any available tick (realtime logs are not subject to rollback).

```pinescript
//@version=6
indicator("Historical and realtime logs demo", "Average bar ratio")

//@variable The current bar's change from the `open` to `close`.
float numerator = close - open
//@variable The current bar's `low` to `high` range.
float denominator = high - low
//@variable The ratio of the bar's open-to-close change to its full range.
float ratio = numerator / denominator
//@variable The average ratio.
float average = ta.sma(ratio, 10)

// Plot the `average`.
plot(average, "average", color.purple, 3)

if barstate.isconfirmed
    switch denominator
        0.0 => log.error("Division by 0 on confirmed bar!\nBar excluded from the average.")
        => log.info(
            "Values (Confirmed):\nnumerator: {0,number,#.########}\ndenominator: {1,number,#.########}\nratio: {2,number,#.########}\naverage: {3,number,#.########}",
            numerator, denominator, ratio, average
        )
else
    switch denominator
        0.0 => log.error("Division by 0 on unconfirmed bar!")
        => log.warning(
            "Values (unconfirmed):\nnumerator: {0,number,#.########}\ndenominator: {1,number,#.########}\nratio: {2,number,#.########}\naverage: {3,number,#.########}",
            numerator, denominator, ratio, average
        )
```

### Inspecting Logs
Logs are automatically prefixed with an ISO 8601 timestamp in the chart's time zone. When hovering over a log in the pane, two options are available:
*   **Source code**: Highlights the code line in the Pine Editor containing the log call.
*   **Scroll to bar**: Navigates the chart to the bar index where the log event occurred, showing a temporary label.

### Filtering Logs
The Pine Logs pane features filter options accessible at the top right of the pane:
*   **Logging Level**: Filter by "Info", "Warning", or "Error".
*   **Start Date**: Display messages starting from a specific date/time.
*   **Search**: Find matching characters or regular expressions.
    *   *Match Case*: Case-sensitive searching.
    *   *Whole Word*: Restricts matches to complete words.
    *   *Regex*: Supports advanced patterns (e.g. `average:\s*(?:0\.5\d*[1-9]\d*|0\.[6-9]\d*|(?:1\.0*|1))`).

---

## Custom Code Filters

Programmers can control `log.*()` generation dynamically using inputs:

```pinescript
//@version=6
indicator("Custom code filters demo", overlay = true)

int lengthInput = input.int(20, "Length", 2)
bool filterLogsInput = input.bool(true, "Only log in time range", group = "Log filter")
int logStartInput = input.time(0, "Start time", group = "Log filter", confirm = true)
int logEndInput = input.time(0, "End time", group = "Log filter", confirm = true)

float rma = ta.rma(close, lengthInput)
bool priceBelow = close <= rma
bool priceRising = close > math.max(hl2[1], close[1])
bool rmaAccelerating = rma - 2.0 * rma[1] + rma[2] > 0.0
bool closeAtThreshold = rma - close > ta.atr(lengthInput) * 2.0
bool compoundCondition = priceBelow and priceRising and rmaAccelerating and closeAtThreshold

plot(rma, "RMA", color=color.teal, linewidth=3)
bgcolor(compoundCondition ? color.new(color.aqua, 80) : na, title = "Compound condition highlight")

bool showLog = filterLogsInput ? time >= logStartInput and time <= logEndInput : true

if barstate.isconfirmed and showLog
    log.info(
        "\nclose:             {0,number,#.#####}\nrma:               {1,number,#.#####}\npriceBelow:        {2}\npriceRising:       {3}\nrmaAccelerating:   {4}\ncloseAtThreshold:  {5}\n\ncompoundCondition: {6}",
        close, rma, priceBelow, priceRising, rmaAccelerating, closeAtThreshold, compoundCondition
    )
```

---

## Pine Drawings (Labels and Tables)

Pine's drawing types can place visual marks at specified locations during code executions.
*   **Labels**: Anchored to bar coordinates, displaying text/tooltips.
*   **Tables**: Rendered in a fixed screen position (e.g., `position.top_right`) unaffected by chart scrolling/zooming.

### Label Debugging on Successive Bars
```pinescript
//@version=6
indicator("Drawing on successive bars demo", "Average bar ratio")

float numerator = close - open
float denominator = high - low
float ratio = numerator / denominator
float average = ta.sma(ratio, 10)

plot(average, "average", color.purple, 3)

if barstate.isconfirmed
    if denominator == 0
        string debugText = "Division by 0 on confirmed bar!\nBar excluded from the average."
        label.new(bar_index, high, debugText, color = color.red, textcolor = #000000, force_overlay = true)
    else
        string debugText = str.format(
            "Values (Confirmed):\nnumerator: {0,number,#.########}\ndenominator: {1,number,#.########}\nratio: {2,number,#.########}\naverage: {3,number,#.########}",
            numerator, denominator, ratio, average
        )
        label.new(bar_index, high, debugText, textcolor = #ffffff, force_overlay = true)
else
    if denominator == 0
        string debugText = "Division by 0 on unconfirmed bar!"
        label.new(bar_index, high, debugText, color = color.red, textcolor = #000000, force_overlay = true)
    else
        string debugText = str.format(
            "Values (unconfirmed):\nnumerator: {0,number,#.########}\ndenominator: {1,number,#.########}\nratio: {2,number,#.########}\naverage: {3,number,#.########}",
            numerator, denominator, ratio, average
        )
        label.new(bar_index, high, debugText, color = color.orange, textcolor = #000000, force_overlay = true)
```

> [!NOTE]
> To avoid overlapping text cluttering the screen, you can assign text to the `tooltip` parameter of `label.new()` instead. The values will only show when you hover your mouse pointer over the drawing element.

### Drawing in Visible Chart Ranges
You can restrict labels to the visible section of your screen to prevent garbage collection limits (max 500 labels) from hiding historical calculations as you scroll.
```pinescript
if time >= chart.left_visible_bar_time and time <= chart.right_visible_bar_time
    // Draw label
```

### Debugging with Tables
```pinescript
//@version=6
indicator("Debugging with single-cell tables demo", "Chart info", true, behind_chart = false)

printTable(string info, simple color textColor = na, simple int size = 18) =>
    var color col    = nz(textColor, chart.fg_color)
    var table result = table.new(position.top_right, 1, 1, na, force_overlay = true)
    table.cell(result, 0, 0, info, text_color = col, text_size = size)

if barstate.islast
    printTable(
        str.format(
            "Chart info:\n\nSymbol: {0}, Type: {1}, Timeframe: {2}\nStandard chart: {3}\n\nO: {4,number,#.#####}, H: {5,number,#.#####}, L: {6,number,#.#####}, C: {7,number,#.#####}\nTotalBars: {8}",
            ticker.standard(), syminfo.type, timeframe.period, chart.is_standard, open, high, low, close, bar_index + 1
        )
    )
```

---

## Plots and Scale Management

Plots display calculations across all bars, but can skew the chart's price axis if their ranges vary significantly.

### Plotting Without Affecting Scale
To plot debugging variables without distorting your indicator's price scale, adjust the `display` parameter to exclude the pane:
```pinescript
//@version=6
indicator("Plotting without affecting the scale demo", "Weighted average", true, precision = 5)

int lengthInput = input.int(20, "Length", 1)
float weight = math.pow(close - open, 2)
float numerator = math.sum(weight * close, lengthInput)
float denominator = math.sum(weight, lengthInput)
float average = numerator / denominator

plot(average, "Weighted average", linewidth = 3)

// Create debug plots that show only in the Status Line and Data Window
debugLocations = display.all - display.pane
plot(weight,      "weight",      color=color.purple, display = debugLocations)
plot(numerator,   "numerator",   color=color.teal,   display = debugLocations)
plot(denominator, "denominator", color=color.maroon, display = debugLocations)
```

---

## Decomposing Expressions

Splitting multi-calculation formulas or nested logical conditions into separate variables makes checking values, profiling code, and finding structural bugs significantly simpler.

```pinescript
//@version=6
strategy("Decomposing expressions demo")

int length1Input = input.int(20)
int length2Input = input.int(40)
int smoothingInput = input.int(10)
int maxLength = math.max(length1Input, length2Input)

// Decomposed calculations
float ema1 = ta.ema(close, length1Input)
float ema2 = ta.ema(close, length2Input)
float diff1 = close - ema1
float diff2 = close - ema2
float change1 = ta.change(diff1, length1Input)
float change2 = ta.change(diff2, length2Input)
float medChange = math.avg(change1, change2)
float osc = ta.ema(medChange, smoothingInput)

float oscDev  = 2 * ta.stdev(osc, maxLength)
float pastAvg = math.avg(osc[1], osc[2])
bool cond1 = osc < oscDev
bool cond2 = osc > 0
bool cond3 = pastAvg < osc

bool upSignal = cond1 and cond2 and cond3

plot(osc, "Custom oscillator", upSignal ? color.aqua : color.gray, style = plot.style_columns)

if upSignal
    strategy.entry("Buy", strategy.long)
else
    strategy.close("Buy")
```

---

## Extracting Data from Local Scopes

Since identifiers inside local scopes (functions, conditional blocks, loops) are inaccessible globally, you must extract their data.

### Extraction via Return Tuples
```pinescript
//@version=6
indicator("Extraction using return expressions demo", overlay = true)

int lengthInput = input.int(50, "Length", 2)

customMA(float source, int length) =>
    var float result = na
    float q1 = ta.percentile_linear_interpolation(source, length, 25)
    float q3 = ta.percentile_linear_interpolation(source, length, 75)
    float outerRange = 0.0
    
    // Extract local variables from if block scope via tuple return
    [upper, lower] = if not na(source)
        float upperRange = source - q3
        float lowerRange = q1 - source
        outerRange := math.max(upperRange, lowerRange, 0.0)
        [upperRange, lowerRange]
        
    float totalRange = ta.range(source, length)
    float alpha = 0.5 * outerRange / totalRange
    result := (1.0 - alpha) * nz(result, source) + alpha * source
    // Return all debug targets in a tuple
    [result, q1, q3, upper, lower, outerRange, totalRange, alpha]

[maValue, q1Dbg, q3Dbg, upperDbg, lowerDbg, outerRangeDbg, totalRangeDbg, alphaDbg] = customMA(close, lengthInput)

plot(maValue, "Custom MA", color.blue, 3)
```

### Extraction via Reference Types (Maps / UDTs)
Because functions can access but not reassign outer fundamental-type variables, use reference types (like maps) to write values out of local scope.
```pinescript
//@version=6
indicator("Extraction using reference types demo", overlay = true)

int lengthInput = input.int(50, "Length", 2)
// Global map
var map<string, float> debugData = map.new<string, float>()

customMA(float source, int length) =>
    var float result = na
    float q1 = ta.percentile_linear_interpolation(source, length, 25)
    debugData.put("q1", q1)
    float q3 = ta.percentile_linear_interpolation(source, length, 75)
    debugData.put("q3", q3)
    
    float outerRange = 0.0
    if not na(source)
        float upperRange = source - q3
        debugData.put("upperRange", upperRange)
        float lowerRange = q1 - source
        debugData.put("lowerRange", lowerRange)
        outerRange := math.max(upperRange, lowerRange, 0.0)
        debugData.put("outerRange", outerRange)
        
    float totalRange = ta.range(source, length)
    debugData.put("totalRange", totalRange)
    float alpha = 0.5 * outerRange / totalRange
    debugData.put("alpha", alpha)
    
    result := (1.0 - alpha) * nz(result, source) + alpha * source
    result

float maValue = customMA(close, lengthInput)
plot(maValue, "Custom MA", color.blue, 3)
```

---

## Debugging Collections and UDTs

Maps and UDTs do not have default string representation methods. You can print them by extracting individual fields or printing array representations.

### UDT Field Extraction
```pinescript
//@version=6
indicator("Debugging objects of UDTs demo")

type Data
    array<float> prices
    array<int>   times
    int          sampleMult

int seedInput = input.int(1234, "Seed", 1)
var Data data = Data.new(array.new<float>(10), array.new<int>(10), int(math.random(1, 11, seedInput)))

if bar_index % data.sampleMult == 0
    data.prices.push(close)
    data.times.push(time)
    data.prices.shift()
    data.times.shift()

    // Debugging by accessing fields and converting arrays to strings
    string fString = "Data object fields:\n\nprices: {0}\n\ntimes: {1}\n\nsampleMult: {2}\n-------" 
    if barstate.isconfirmed
        log.info(fString, str.tostring(data.prices), str.tostring(data.times), data.sampleMult)
```
