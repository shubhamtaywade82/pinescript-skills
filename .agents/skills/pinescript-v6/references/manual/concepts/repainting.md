# Repainting

Repainting is defined as script behavior that causes historical calculations or plots to behave differently from realtime calculations. 

Not all repainting is inherently bad or misleading. For example, volume profile indicators, MACD, and RSI naturally update on realtime bars until the bars close. However, programmers must understand how repainting occurs to prevent writing misleading scripts—such as those that leak future data into the past or generate orders based on non-standard chart types.

---

## Types of Repainting Behavior

1.  **Widespread & Acceptable**: Fluid calculations on an unconfirmed realtime bar (e.g. using `close` to track current price). These values fluctuate until the bar closes and becomes a historical bar.
2.  **Potentially Misleading**: Scripts plotting values into the past, pivot points updating retrospectively, or strategies using `calc_on_every_tick = true` that cannot replicate order logic historically.
3.  **Unacceptable**: Leaking future data into the past using `lookahead` without offsets, or executing backtests on synthetic charts (Renko, Kagi, Point & Figure) which yields unrealistic performance metrics.
4.  **Unavoidable**: Exchange adjustments or stock splits altering historical databases, or differences in the starting bar index due to user plan limits.

---

## Historical vs. Realtime Calculations

### 1. Fluid Data Values
Historical bars only contain static open, high, low, and close (OHLC) values. Realtime bars are fluid; the high, low, and close values fluctuate on every tick.

#### Repainting Example
This script colors the background green or red on moving average crossovers. Because `close` fluctuates on the realtime bar, the background color can repaint:
```pinescript
//@version=6
indicator("Repainting MA Cross", "", true)
ma = ta.ema(close, 5)
xUp = ta.crossover(close, ma)
xDn = ta.crossunder(close, ma)
plot(ma, "MA", color.black, 2)
bgcolor(xUp ? color.new(color.lime, 80) : xDn ? color.new(color.fuchsia, 80) : na)
```

#### Non-Repainting Alternatives
*   **Method A**: Check `barstate.isconfirmed` to wait until the realtime bar is finalizing:
    ```pinescript
    xUp = ta.crossover(close, ma) and barstate.isconfirmed
    ```
*   **Method B**: Shift the crossover conditions to previous closed bars:
    ```pinescript
    xUp = ta.crossover(close, ma)[1]
    ```
*   **Method C**: Base calculations entirely on confirmed data:
    ```pinescript
    ma = ta.ema(close[1], 5)
    xUp = ta.crossover(close[1], ma)
    ```

---

## Repainting in `request.security()`

### 1. Higher-Timeframe (HTF) Fluctuation
`request.security()` retrieves unconfirmed developing data on realtime bars by default. When the script restarts, these bars are parsed using finalized historical data, causing differences.

#### Avoiding HTF Repainting
To get consistent results on historical and realtime bars, offset the expression by `[1]` and enable `lookahead = barmerge.lookahead_on`:

```pinescript
//@version=6
indicator("HTF Security Non-Repainting Demo", overlay = true)

string higherTimeframe = input.timeframe("30", "Timeframe")

// Repainting: gets current developing close
repaintingClose = request.security(syminfo.tickerid, higherTimeframe, close)

// Non-Repainting: requests last confirmed close via lookahead + offset
nonRepaintingClose = request.security(syminfo.tickerid, higherTimeframe, close[1], lookahead = barmerge.lookahead_on)

plot(repaintingClose, "Repainting Close", color = color.new(color.purple, 50), linewidth = 4)
plot(nonRepaintingClose, "Non-Repainting Close", color = color.teal, linewidth = 2)
```

#### Handy Non-Repainting Helper Function
```pinescript
noRepaintSecurity(symbol, timeframe, expression) =>
    request.security(symbol, timeframe, expression[1], lookahead = barmerge.lookahead_on)
```

### 2. Future Leaks
Setting `lookahead = barmerge.lookahead_on` without offsetting the series by `[1]` causes a future leak. The script will know the high/low of an HTF bar before it historically closed:

```pinescript
// FUTURE LEAK! DO NOT USE in indicators or strategies
//@version=6
indicator("Future leak", "", true)
futureHigh = request.security(syminfo.tickerid, "1D", high, lookahead = barmerge.lookahead_on)
plot(futureHigh)
```
*Note*: Public publications that contain future leaks are subject to moderation.

---

## Other Causes of Repainting

### 1. `varip`
Variables declared with `varip` retain values between realtime tick updates. Because historical data does not contain tick records, calculations using `varip` cannot be reconstructed on historical bars.

### 2. Bar State Built-ins
Using `barstate.isnew` can cause discrepancies. On historical bars, `barstate.isnew` is evaluated as true on the bar's *close* iteration, whereas in realtime, it is true on the bar's *open* tick.

### 3. `timenow`
The `timenow` variable returns the current system clock time. Since this value changes constantly, scripts using it will produce different plots historically.

---

## Plotting in the Past

Indicators that identify historical occurrences, like pivots, require a lookback window before confirmation (e.g. `ta.pivothigh(5, 5)` requires 5 bars to elapse). Plotting these events retroactively creates a visual indicator that looks highly predictive but is actually lagging:

```pinescript
//@version=6
indicator("Plotting in the past", "", true)
plotInThePast = input.bool(false, "Plot in the past (retrospective)")

pHi = ta.pivothigh(5, 5)

if not na(pHi)
    // Shift coordinate by 5 index places if plotting in the past is enabled
    label.new(bar_index[plotInThePast ? 5 : 0], na, str.tostring(pHi, format.mintick), yloc = yloc.abovebar)
```

---

## Dataset Variations

*   **Starting Points**: Indicators like `ta.ema()`, `ta.valuewhen()`, or `ta.barssince()` carry calculations forward sequentially from the very first bar in the dataset (`bar_index == 0`). If a chart loads fewer bars due to user plan limits, the calculations will start at a different point in time, leading to slightly different terminal values.
*   **Data Revision**: Exchanges occasionally update historical datasets to retroactively account for splits or trade adjustments, which can result in minor calculation differences upon reloading charts.
