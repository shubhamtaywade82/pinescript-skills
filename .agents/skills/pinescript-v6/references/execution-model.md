# Pine Script® v6 Execution Model Reference

Understanding how TradingView compiles and executes Pine Script code is critical to building indicators and strategies that calculate correctly and run efficiently.

## 1. Bar-by-Bar Execution

Pine Script executes on charts chronologically, from the oldest historical bar to the newest real-time tick.
- The script is called once for every bar on the chart.
- Variables declared without `var` or `varip` are re-initialized on every single bar.
- On historical bars, the script executes once at the close of the bar.
- On real-time bars (the current active bar), the script executes on every price tick.

---

## 2. Variable Persistence: `var` and `varip`

### `var` Keyword
Variables declared with `var` are initialized only once on the first bar of the chart. Their values persist across bars.
- On real-time bars, if a variable modified with `var` is changed during a tick, its value is **rolled back** to the value it had at the close of the previous bar if the tick is updated, and is only finalized when the bar closes.
```pinescript
// Counts the total number of bars on the chart
var int barCount = 0
barCount := barCount + 1
```

### `varip` Keyword (Var Intra-Bar)
Variables declared with `varip` persist across bars *and* do **not** roll back between ticks on a real-time bar. Their values accumulate tick-by-tick.
- Used for tracking intra-bar data, such as counting ticks or measuring time between updates.
```pinescript
// Counts the number of ticks that occur during a single bar
varip int tickCount = 0
if barstate.isnew
    tickCount := 0
tickCount := tickCount + 1
```

---

## 3. Barstate Variables

Barstate flags allow the script to determine which phase of execution it is currently running in.

| Variable | Description |
| :--- | :--- |
| **`barstate.ishistory`** | True on all historical bars (bars that have already closed). |
| **`barstate.isrealtime`** | True on the active, real-time bar. |
| **`barstate.isnew`** | True on the first tick/execution of a new bar. |
| **`barstate.islast`** | True on the very last bar (real-time bar, or the last historical bar if chart has no real-time data). |
| **`barstate.islastconfirmedhistory`** | True on the last historical bar that has closed. Excellent for drawing static visual elements. |

---

## 4. Bar Indexes and History

### `bar_index`
- The `bar_index` variable represents the index of the current bar (starts at `0` for the oldest bar on the chart).
- Accessible to trace how many bars the chart contains.

### Lookback Limit
- The historical operator `[]` lets you look back in time (e.g., `close[1]` is the close of the previous bar).
- **Max Bars Back**: TradingView allocates buffer memory for history dynamically. If the compiler cannot determine how far back a variable is referenced, it may crash. You can resolve this by adding `max_bars_back` in your indicator/strategy declaration:
  ```pinescript
  indicator("My Script", max_bars_back = 500)
  ```
