# Inputs

Inputs allow users to customize variables in a script's "Settings/Inputs" tab without editing the source code. Modifying input values causes the script to re-execute on all historical and realtime data.

Our Style Guide recommends placing all `input.*()` declarations at the beginning of the script.

---

## Input Functions Overview

Pine Script® features the following input functions:
*   `input()`: Generic input (automatically infers type from default value).
*   `input.int()`: Integer numeric inputs.
*   `input.float()`: Float numeric inputs.
*   `input.bool()`: Boolean checkbox toggles.
*   `input.color()`: Color selection widgets.
*   `input.string()`: Monoline text fields or dropdown menus.
*   `input.text_area()`: Multiline text fields for alerts/parameter lists.
*   `input.timeframe()`: Dropdown timeframe configurations.
*   `input.symbol()`: Symbol Search input referencing tickers.
*   `input.source()`: Price values or outputs from other indicator plots.
*   `input.session()`: Hours and trading session ranges (e.g. `0600-1700`).
*   `input.time()`: Calendar dates and timestamps.
*   `input.price()`: Interactive horizontal price levels.
*   `input.enum()`: Strict dropdown options mapping to custom `enum` types.

Values returned by input functions (except for `input.source()`) have the `input` type qualifier. They cannot be used where `const` values are strictly required, but they are stronger qualifiers than standard `series` values.

---

## Common Function Parameters

*   **`defval`**: The default fallback value. The variable is initialized to this.
*   **`title`**: Label shown next to the widget. Defaults to the variable name if omitted.
*   **`tooltip`**: Optional string displaying a hoverable info icon. Supports `\n` line breaks.
*   **`inline`**: Calls sharing the exact same string argument will be placed side-by-side on the same line.
*   **`group`**: Group heading section separating categories.
*   **`display`**: Controls visibility in status lines/data windows (`display.all`, `display.status_line`, `display.data_window`, `display.none`).
*   **`active`**: An `input bool` expression indicating whether the widget is enabled (if false, it is grayed out).
*   **`options`**: List of values wrapped in brackets (`[val1, val2]`) displaying a dropdown of predefined options.
*   **`confirm`**: If `true`, the indicator prompts the user with an input dialog before plotting on the chart.

---

## Core Input Types

### Generic Input
```pinescript
//@version=6
indicator("`input()` demo", overlay = true)
a = input(1, "input int")
b = input(1.0, "input float")
c = input(true, "input bool")
d = input(color.orange, "input color")
e = input("1", "input string")
f = input(close, "series float (source)")
```

### Numeric & Boolean Inputs (Bollinger Bands Example)
This example highlights using `inline` parameters to place checkbox toggles alongside numeric settings:
```pinescript
//@version=6
indicator("MA with BB", "", true)

maLengthInput = input.int(10,     "MA length", minval = 1)
bbFactorInput = input.float(1.5,  "BB factor", inline = "01", minval = 0, step = 0.5)
showBBInput   = input.bool(true,  "Show BB",   inline = "01")

ma      = ta.sma(close, maLengthInput)
bbWidth = ta.stdev(ma, maLengthInput) * bbFactorInput
bbHi    = ma + bbWidth
bbLo    = ma - bbWidth

plot(ma, "MA", color.aqua)
plot(showBBInput ? bbHi : na, "BB Hi", color.gray)
plot(showBBInput ? bbLo : na, "BB Lo", color.gray)
```

### Color Inputs
```pinescript
//@version=6
indicator("Color Modulated Bands", "", true)

maLengthInput = input.int(10,           "MA length", inline = "01", minval = 1)
maColorInput  = input.color(color.aqua, "",          inline = "01")
bbFactorInput = input.float(1.5,        "BB factor", inline = "02", minval = 0, step = 0.5)
bbColorInput  = input.color(color.gray, "",          inline = "02")
showBBInput   = input.bool(true,        "Show BB",   inline = "02")

ma      = ta.sma(close, maLengthInput)
bbWidth = ta.stdev(ma, maLengthInput) * bbFactorInput
bbHi    = ma + bbWidth
bbLo    = ma - bbWidth

bbHiColor = color.new(bbColorInput, high > bbHi ? 60 : 0)
bbLoColor = color.new(bbColorInput, low  < bbLo ? 60 : 0)

plot(ma, "MA", maColorInput)
plot(showBBInput ? bbHi : na, "BB Hi", bbHiColor, 2)
plot(showBBInput ? bbLo : na, "BB Lo", bbLoColor, 2)
```

---

## Specialized Input Types

### String & Text Area Inputs
*   `input.string()` is ideal for short options or dropdowns. It treats `\` backslashes as literal characters (no escape sequences like `\n` are parsed inside the widget).
*   `input.text_area()` creates a larger multiline text field, suited forAlert/Webhooks formatting or comma-separated configurations.

```pinescript
//@version=6
indicator("Text Area splitting demo", overlay = true)

string symbolListInput = input.text_area("AAPL,GOOG,NVDA,MSFT", "Symbol list")
var array<string> symbols = str.split(symbolListInput, ",")

if barstate.islast
    var table display = table.new(position.bottom_right, 2, symbols.size())
    for [i, symbol] in symbols
        display.cell(0, i, symbol, text_color = chart.fg_color, text_size = 20)
        float vol = request.security(symbol, "", volume)
        display.cell(1, i, str.tostring(vol, format.volume), text_color = chart.fg_color, text_size = 20)
```

### Timeframe & Symbol Inputs
Use these to fetch historical details from alternative tickers or resolutions securely:
```pinescript
//@version=6
indicator("Symbol and Timeframe request demo", overlay = false)

string tfInput     = input.timeframe("1D", "Target timeframe")
string symbolInput = input.symbol("", "Target symbol")

// Retrieve RSI dynamically
float symbolRSI = request.security(symbolInput, tfInput, ta.rsi(close, 14))

plot(symbolRSI, "RSI", color.aqua)
```

### Time & Price Interactive Inputs
`input.time()` parses dates into millisecond UNIX timestamps. `input.price()` provides a horizontal handle. 

When a time input and price input share the exact same `group` and `inline` parameters, they are combined into an **interactive coordinate point** directly on the chart:
```pinescript
//@version=6
indicator("Price and time point demo", overlay = true)

float price1Input = input.price(0, "Price 1", inline = "1", confirm = true)
int   time1Input  = input.time(0,  "Time 1",  inline = "1", confirm = true)
float price2Input = input.price(0, "Price 2", inline = "2", confirm = true)
int   time2Input  = input.time(0,  "Time 2",  inline = "2", confirm = true)

var array<chart.point> points = array.from(
     chart.point.from_time(time1Input, price1Input),
     chart.point.from_time(time2Input, price2Input)
 )

if barstate.islast
    polyline.new(points, curved = true, xloc = xloc.bar_time, line_color = color.purple, line_width = 3)
```

### Enum Dropdowns
Using `input.enum()` enforces type-safe choices from custom `enum` types:
```pinescript
//@version=6
indicator("Enum input demo", overlay = true)

enum SignalType
    long  = "Only long signals"
    short = "Only short signals"
    both  = "Long and short signals"
    none 

SignalType sigInput = input.enum(SignalType.long, "Signal type")

ma1 = ta.sma(ohlc4, 10)
ma2 = ta.sma(ohlc4, 200)

bool longCross  = ta.crossover(close, math.max(ma1, ma2))
bool shortCross = ta.crossunder(close, math.min(ma1, ma2))

bool longSignal  = (sigInput == SignalType.long  or sigInput == SignalType.both) and longCross
bool shortSignal = (sigInput == SignalType.short or sigInput == SignalType.both) and shortCross

plotshape(longSignal,  "Long signal",  shape.triangleup,   location.belowbar, color.teal)
plotshape(shortSignal, "Short signal", shape.triangledown, location.abovebar, color.maroon)
```

---

## Formatting Aligned Labels

Using default strings might lead to mismatched vertical layouts in the Inputs panel. To format widgets, pad the `title` arguments with Unicode **EN spaces** (`U+2002`):

```pinescript
//@version=6
indicator("Aligned inputs demo", overlay = true)

// Padding using EN Spaces: "MA source   "
maSourceInput   = input(close, "MA source   ",  inline = "21")
maLengthInput   = input.int(14, "Length",        inline = "21")
longSourceInput = input(close, "Signal source", inline = "22")
longLengthInput = input.int(14, "Length",        inline = "22")
```
