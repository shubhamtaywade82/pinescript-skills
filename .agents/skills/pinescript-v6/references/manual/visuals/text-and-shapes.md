# Text and Shapes

Pine Script® features five different ways to display text or shapes on the chart:
1.  `plotchar()`
2.  `plotshape()`
3.  `plotarrow()`
4.  **Labels** created with `label.new()`
5.  **Tables** created with `table.new()`

---

## Choosing the Right Tool

*   **Tables**: Float in fixed screen locations, independent of chart zoom or horizontal scrolling. Contents do not align with specific bars.
*   **plotshape() & plotchar()**: Display static text string identifiers on every bar where a condition evaluates to true. Text must be compile-time constants (`const string`).
*   **plotarrow()**: Plots up/down arrows whose size corresponds proportionally to the value of a series. Cannot display text.
*   **Labels**: Display dynamic (`series string`) text anywhere on the chart, aligned to price and time coordinates. Text can contain dynamic expressions like price values converted via `str.tostring()`. Limited to a maximum of 500 active labels on the chart.

### Shared Typography Rules
*   String concatenation is achieved using the `+` operator.
*   Use `\n` to insert line breaks inside labels, boxes, tables, and `plotshape()` text.
*   Formatting (e.g. bold, italics, monospace) is supported on drawing types (labels, tables, boxes) but not on raw `plot*()` shapes.

```pinescript
//@version=6
indicator("Four displays of text", overlay = true)

plotchar(ta.rising(close, 5), "`plotchar()`", "🠅", location.belowbar, color.lime, size = size.small)
plotshape(ta.falling(close, 5), "`plotshape()`", location = location.abovebar, color = na, text = "•`plotshape()•`\n🠇", textcolor = color.fuchsia, size = size.huge)

if bar_index % 25 == 0
    label.new(bar_index, na, "•LABEL•\nHigh = " + str.tostring(high, format.mintick) + "\n🠇", yloc = yloc.abovebar, style = label.style_none, textcolor = color.black, size = size.normal)

printTable(txt) => 
    var table t = table.new(position.middle_right, 1, 1)
    table.cell(t, 0, 0, txt, bgcolor = color.yellow)
    
printTable("•TABLE•\n" + str.tostring(bar_index + 1) + " bars\nin the dataset")
```

---

## `plotchar()`

The signature of `plotchar()` is:
```pinescript
plotchar(series, title, char, location, color, offset, text, textcolor, editable, size, show_last, display, format, precision, force_overlay) → void
```

Use `plotchar()` to plot a single character on specific bars or track variable values in the Data Window without distorting the price scale (by setting `location = location.top` and using empty character displays):
```pinescript
//@version=6
indicator("Data window check", overlay = true)
plotchar(bar_index, "Bar index", "", location = location.top)
```

For conditional signals, display the character only when conditions evaluate to true:
```pinescript
//@version=6
indicator("Signals", overlay = true)
bool longSignal = ta.rising(close, 2) and ta.rising(high, 2) and (na(volume) or ta.rising(volume, 2))
plotchar(longSignal, "Long Signal", "▲", location = location.belowbar, color = na(volume) ? color.gray : color.blue, size = size.tiny)
```

---

## `plotshape()`

Plots shapes with accompanying text strings. 

The signature of `plotshape()` is:
```pinescript
plotshape(series, title, style, location, color, offset, text, textcolor, editable, size, show_last, display, format, precision, force_overlay) → void
```

To prevent text overlaps, use line breaks. For example, insert `\n` at the end of the text for shapes plotted above bars, or at the start of the text for shapes below bars:
```pinescript
//@version=6
indicator("Lift text", "", true)
plotshape(true, "", shape.arrowup,   location.abovebar, color.green,  text = "A")
plotshape(true, "", shape.arrowup,   location.abovebar, color.lime,   text = "B\n")
plotshape(true, "", shape.arrowdown, location.belowbar, color.red,    text = "C")
plotshape(true, "", shape.arrowdown, location.belowbar, color.maroon, text = "\nD")
```

### Available Shape Styles
*   `shape.xcross`, `shape.cross`
*   `shape.circle`, `shape.square`, `shape.diamond`
*   `shape.triangleup`, `shape.triangledown`
*   `shape.arrowup`, `shape.arrowdown`
*   `shape.flag`
*   `shape.labelup`, `shape.labeldown`

---

## `plotarrow()`

Renders proportional arrows above or below bars based on a numerical series:
*   `series > 0`: Upward arrow with length proportional to the value.
*   `series < 0`: Downward arrow.
*   `series == 0` or `na`: No arrow is plotted.

```pinescript
//@version=6
indicator("Body size arrows", overlay = true)
body = close - open
plotarrow(body, colorup = color.teal, colordown = color.orange)
```

---

## Labels (`label.new()`)

Labels are drawing objects referenced using pointers. 

The signature of `label.new()` supports two overloads:
```pinescript
label.new(point, text, xloc, yloc, color, style, textcolor, size, textalign, tooltip, text_font_family, force_overlay, text_formatting) → series label
label.new(x, y, text, xloc, yloc, color, style, textcolor, size, textalign, tooltip, text_font_family, force_overlay, text_formatting) → series label
```

### Displaying Highest High Value in a Window
This script maintains a single persistent label using the `var` keyword, moving it and updating its coordinates when a new high is detected:
```pinescript
//@version=6
indicator("Moving label", overlay = true)

LOOKBACK = 50
hi = ta.highest(LOOKBACK)
highestBarOffset = - ta.highestbars(LOOKBACK)

// Create label once on bar zero
var lbl = label.new(na, na, "", color = color.orange, style = label.style_label_lower_left)

// Update properties only when the highest high changes
if ta.change(hi) != 0
    labelText = "High: " + str.tostring(hi, format.mintick)
    tooltipText = "Offset: " + str.tostring(highestBarOffset) + "\nLow: " + str.tostring(low[highestBarOffset], format.mintick)
    
    label.set_xy(lbl, bar_index[highestBarOffset], hi)
    label.set_text(lbl, labelText)
    label.set_tooltip(lbl, tooltipText)
```

### Position Properties
*   **`x`**: Bar index or UNIX time timestamp. Supports future offsets up to 500 bars and past offsets up to 10,000 bars.
*   **`yloc`**: Anchoring coordinates: `yloc.price` (references `y` price), `yloc.abovebar`, or `yloc.belowbar` (ignores `y`).
*   **`style`**: Bubble shapes: e.g. `label.style_label_up`, `label.style_label_down`, `label.style_none`.

### Deleting Labels
To restrict the number of visible labels, delete the oldest entries stored inside the `label.all` array:
```pinescript
if array.size(label.all) > qtyLabelsInput
    label.delete(array.get(label.all, 0))
```

To display a label **only on the final bar**, write it efficiently inside a `barstate.islast` block using a setter instead of deleting on every bar:
```pinescript
//@version=6
indicator("Last bar label", overlay = true)

if barstate.islast
    var lbl = label.new(na, na)
    label.set_xy(lbl, bar_index, high)
    label.set_text(lbl, str.tostring(high, format.mintick))
```

---

## Text Formatting

Fitted fonts, size scales, and style overlays can be assigned dynamically to drawings (labels, tables, boxes):

### Typographic Emphasis Constants
*   `text.format_none` (Default)
*   `text.format_bold`
*   `text.format_italic`
*   `text.format_bold + text.format_italic`

### Sizing Conversions

| Sizing Constant | Table / Box Size (int) | Label Size (int) |
| :--- | :--- | :--- |
| `size.auto` | 0 | 0 |
| `size.tiny` | 8 | ~7 |
| `size.small` | 10 | ~10 |
| `size.normal` | 14 | 12 |
| `size.large` | 20 | 18 |
| `size.huge` | 36 | 24 |

### Formatting Demo Options
```pinescript
//@version=6
indicator("Text formatting demo", overlay = true)

string closeLabelSize = input.string(size.large, "Label text size", 
     [size.auto, size.tiny, size.small, size.normal, size.large, size.huge])
int tableTextSize = input.int(25, "Table text size", minval = 0)

bool formatBold   = input.bool(false, "Bold")
bool formatItalic = input.bool(true,  "Italic")

float recentHighest = ta.highest(20)
float recentLowest  = ta.lowest(20)

if barstate.islast
    label closeLabel = label.new(bar_index, close, "Close: " + str.tostring(close, "$0.00"), 
         color = #EB9514D8, style = label.style_label_left, size = closeLabelSize)

    float barMove = close - open
    var table barMoveTable = table.new(position.bottom_right, 1, 1, bgcolor = barMove > 0 ? #31E23FCC : #EE4040CC)
    barMoveTable.cell(0, 0, "Move = " + str.tostring(barMove, "$0.00") + "\nPct = " 
         + str.tostring(barMove / open, "0.00%"), text_halign = text.align_right, text_size = tableTextSize)

    box rangeBox = box.new(bar_index - 20, recentHighest, bar_index + 1, recentLowest, text_size = 19,
         bgcolor = #A4B0F826, text_valign = text.align_top, text_color = #4A07E7D8)
         
    rangeBox.set_text("Current is " + 
         (close >= (recentHighest + recentLowest) / 2 ? str.tostring(recentHighest - close, "$0.00") + " from high"
         : str.tostring(close - recentLowest, "$0.00") + " from low"))
    
    // Choose emphasis style dynamically
    int formatStyle = formatBold and formatItalic ? (text.format_bold + text.format_italic) : (formatBold ? text.format_bold : (formatItalic ? text.format_italic : text.format_none))
    
    closeLabel.set_text_formatting(formatStyle)
    barMoveTable.cell_set_text_formatting(0, 0, formatStyle)
    rangeBox.set_text_formatting(formatStyle)
```
