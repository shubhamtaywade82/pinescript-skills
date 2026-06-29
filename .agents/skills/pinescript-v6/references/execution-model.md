# Pine Script® v6 Execution Model Reference

Source: https://www.tradingview.com/pine-script-docs/language/execution-model/
Status: unverified

## Core Overview

Pine Script uses an **event-driven, sequential execution model**:
- A script executes repeatedly across historical bars and realtime ticks.
- After each execution on a closed bar, results are committed to an internal time series.
- The execution model is tightly coupled with the type system.

Declaration statements like `indicator()` or `strategy()` run at compile time, not during bar executions.

## The Basics

### Bar-by-Bar Execution

- A chart dataset is a sequence of bars from earliest to most recent.
- The script executes once per historical bar.
- Built-in bar variables (`open`, `high`, `low`, `close`, `volume`) update before each execution.

#### Variable Declaration Basics

- Variables are re-declared and reinitialized on every execution by default.
- Use `var` to initialize once on the first bar and persist across bars.
- Use `varip` to persist across bars and ticks without rollback.

```pine
//@version=6
indicator("Repeated declarations demo")
int x = 0
x += 10
plot(x, "`x` value", color.blue, 3)
```

```pine
//@version=6
indicator("Persistent declarations demo")
var int x = 0
x += 10
plot(x, "`x` series", color.blue, 3)
```

### Storing and Using Data from Previous Bars

- Use the history-referencing operator `[]` to access past values.
- Use `ta.*()` functions to compute values from internal historical data.

```pine
//@version=6
indicator("Storing and using data from previous bars demo")
float priceReturn = ta.change(close, 1) / close[1]
color returnColor = priceReturn > priceReturn[1] ? color.teal : color.maroon
plot(priceReturn, "Price return", returnColor, 1, plot.style_columns)
```

### Realtime Bars

- Historical bars: closed, confirmed data; script executes once per bar.
- Realtime bars: open, unconfirmed data; indicators execute once per tick to recalculate.
- Before each recalculation, **rollback** resets applicable data to the last committed state from the previous bar’s close.
- Only values from the final tick are committed after the bar closes.

```pine
//@version=6
indicator("Recalculation on realtime bars demo")
int lengthInput = input.int(10, "Length", 1)
float stochastic = ta.stoch(close, high, low, lengthInput)
plot(stochastic, "Stochastic %K", color.teal, 3)
bgcolor(barstate.isrealtime ? color.new(color.purple, 80) : na)
```

## The Details

### Executions on Historical Bars

When a script loads, its compiled code runs from the oldest accessible bar to the newest, then commits outputs to the internal time series. By default:
- Indicators and libraries execute once per historical bar.
- Strategies execute once per bar by default, but can execute more if `calc_on_order_fills = true`.

### Rollback in Detail

- Applies to variables declared without `varip`.
- Reinitializes non-`varip` variables before each new execution.
- Reverts `var` variables to the last committed state from the previous bar.
- Replaces temporary plot outputs (plot*, `bgcolor`, `barcolor`, `fill`) on the current bar.

### Strategy Realtime Behavior

- Default: strategies execute only once per bar at closing tick.
- `calc_on_every_tick = true`: execute after each update on an open bar.
- `calc_on_order_fills = true`: execute on each tick where the broker emulator fills an order.

## barstate Variables

| Variable | Meaning |
|----------|---------|
| `barstate.ishistory` | `true` on all closed historical bars |
| `barstate.isrealtime` | `true` on the active realtime bar |
| `barstate.isnew` | `true` on the first tick of a new bar |
| `barstate.islast` | `true` on the last bar in the dataset |
| `barstate.islastconfirmedhistory` | `true` on the last confirmed historical bar |
| `barstate.isconfirmed` | `true` on confirmed closed bars |
