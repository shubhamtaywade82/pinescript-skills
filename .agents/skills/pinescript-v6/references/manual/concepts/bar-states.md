# Bar States

A set of built-in variables in the `barstate` namespace allows your script to detect different properties of the chart bar on which the script is currently executing. These states can be used to restrict the execution or the logic of your code to specific bars.

> [!NOTE]
> Indicators and libraries run on all price or volume updates in real time. However, strategies **not** using `calc_on_every_tick = true` do not evaluate on every tick; they execute only when the realtime bar closes. This will affect how `barstate` variables behave in strategy code.

---

## Bar State Built-in Variables

### `barstate.isfirst`
True only on the dataset’s first bar (i.e., when `bar_index` is zero).
Useful for initializing array elements or variable structures only once:
```pinescript
FILL_COLOR = color.green
var fillColors = array.new<color>(0)

if barstate.isfirst
    array.push(fillColors, color.new(FILL_COLOR, 70))
    array.push(fillColors, color.new(FILL_COLOR, 75))
    array.push(fillColors, color.new(FILL_COLOR, 80))
```

### `barstate.islast`
True if the current bar is the last bar on the chart, whether that bar is a realtime bar or a historical bar.
Commonly used to restrict drawing calculations (labels, lines, tables) to the final bar for efficiency:
```pinescript
//@version=6
indicator("Last bar label", overlay = true)
var label hiLabel = label.new(na, na, "")

if barstate.islast
    label.set_xy(hiLabel, bar_index, high)
    label.set_text(hiLabel, str.tostring(high, format.mintick))
```

### `barstate.ishistory`
True on all historical bars. It is never true when `barstate.isrealtime` is also true, and it does not become true on a realtime bar’s closing update.

### `barstate.isrealtime`
True if the current data update is a real-time bar update, false otherwise. Note that `barstate.islast` is also true on all realtime bars.

### `barstate.isnew`
True on all historical bars and on the realtime bar’s first (opening) update. 

Because the Pine Script® runtime executes your script sequentially from the first bar to the last, each historical bar is discovered as a "new" bar.
Useful for resetting `varip` variables at the beginning of each realtime bar:
```pinescript
//@version=6
indicator("Realtime update counter")

updateNo() => 
    varip int updateNo = na
    if barstate.isnew
        updateNo := 1
    else
        updateNo += 1
    updateNo

plot(updateNo(), "Updates on current bar")
```

### `barstate.isconfirmed`
True on all historical bars and on the last (closing) update tick of a realtime bar.
Useful for avoiding repainting by requiring the realtime bar to be confirmed (closed) before indicator calculations or trade conditions evaluate to true:
```pinescript
//@version=6
indicator("Non-repainting RSI")
myRSI = ta.rsi(close, 20)
// Plot na on unconfirmed realtime ticks
plot(barstate.isconfirmed ? myRSI : na)
```

> [!WARNING]
> The `barstate.isconfirmed` variable will **not** work when evaluated inside a `request.security()` call.

### `barstate.islastconfirmedhistory`
True if the script is executing on the dataset’s last bar when the market is closed, or on the bar immediately preceding the realtime bar if the market is open.
Allows you to:
*   Detect the transition to the first realtime bar via `barstate.islastconfirmedhistory[1]`.
*   Postpone server-intensive calculations until the last historical bar is reached.

---

## Interactive Bar States Demo

This script places a label above each bar displaying which `barstate` flags evaluate to true:

```pinescript
//@version=6
indicator("Bar States", overlay = true, max_labels_count = 500)

stateText() =>
    string txt = ""
    txt += barstate.isfirst     ? "isfirst\n"     : ""
    txt += barstate.islast      ? "islast\n"      : ""
    txt += barstate.ishistory   ? "ishistory\n"   : ""
    txt += barstate.isrealtime  ? "isrealtime\n"  : ""
    txt += barstate.isnew       ? "isnew\n"       : ""
    txt += barstate.isconfirmed ? "isconfirmed\n" : ""
    txt += barstate.islastconfirmedhistory ? "islastconfirmedhistory\n" : ""
    txt

labelColor = switch
    barstate.isfirst                => color.fuchsia
    barstate.islastconfirmedhistory => color.gray
    barstate.ishistory              => color.silver
    barstate.isconfirmed            => color.orange
    barstate.isnew                  => color.red
    => color.yellow

label.new(bar_index, na, stateText(), yloc = yloc.abovebar, color = labelColor)
```

### Color Coding States Summary:
*   **Fuchsia**: The absolute first bar in the dataset (`bar_index == 0`).
*   **Silver**: Standard historical bars.
*   **Gray**: The last confirmed historical bar before realtime data begins.
*   **Red**: The opening tick of the realtime bar (`barstate.isnew` is true).
*   **Yellow**: All subsequent updates (ticks) on the realtime bar.
*   **Orange**: The closing update of the realtime bar (when it closes and becomes confirmed).
