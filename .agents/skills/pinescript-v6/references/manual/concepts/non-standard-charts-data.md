# Non-Standard Charts Data

Pine Script® features several `ticker.*()` functions that generate ticker identifiers for requesting data from non-standard chart feeds. By using these identifiers inside `request.security()` calls, scripts can retrieve alternative chart data while executing on any standard chart type.

The available functions are:
*   `ticker.heikinashi()`
*   `ticker.renko()`
*   `ticker.linebreak()`
*   `ticker.kagi()`
*   `ticker.pointfigure()`

> [!IMPORTANT]
> Non-standard charts like Renko, Line Break, Kagi, and Point & Figure construct bars by grouping historical price data from lower resolutions. As a result, the values returned only approximate calculations based on raw historical ticks.

---

## Heikin-Ashi (`ticker.heikinashi()`)

Heikin-Ashi candlesticks use synthetic, averaged OHLC prices calculated from combinations of the current and previous bar's real prices. This math dampens market noise, creating smoother visual trends. 

However, Heikin-Ashi prices are **unsuited to backtesting or automated trading** since orders in strategies execute at real market prices, not Heikin-Ashi prices.

### 1. Overlaying Heikin-Ashi Close Price
This script requests the close value of Heikin-Ashi bars and plots it directly over standard candlesticks:
```pinescript
//@version=6
indicator("HA Close", "", true)
haTicker = ticker.heikinashi(syminfo.tickerid)
haClose = request.security(haTicker, timeframe.period, close)
plot(haClose, "HA Close", color.black, 3)
```

### 2. Requesting Data Without Extended Hours
To request Heikin-Ashi data strictly for regular trading hours, construct an intermediate ticker identifier omitting extended sessions using `ticker.new()`:
```pinescript
//@version=6
indicator("HA Regular Hours Close", "", true)

// Create a custom ticker with regular trading session parameters
regularSessionTicker = ticker.new(syminfo.prefix, syminfo.ticker, session.regular)
haTicker = ticker.heikinashi(regularSessionTicker)

// Request data using gaps_on to receive na outside of regular hours
haClose = request.security(haTicker, timeframe.period, close, gaps = barmerge.gaps_on)

// Use style_linebr to break the plot line on na gaps
plot(haClose, "HA Close", color.black, 3, plot.style_linebr)
```

### 3. Plotting Heikin-Ashi Candles (Sub-pane)
Retrieve Heikin-Ashi OHLC values simultaneously using a tuple inside the request call:
```pinescript
//@version=6
indicator("Heikin-Ashi candles")
CANDLE_GREEN = #26A69A
CANDLE_RED   = #EF5350

haTicker = ticker.heikinashi(syminfo.tickerid)
[haO, haH, haL, haC] = request.security(haTicker, timeframe.period, [open, high, low, close])

candleColor = haC >= haO ? CANDLE_GREEN : CANDLE_RED
plotcandle(haO, haH, haL, haC, color = candleColor)
```

---

## Renko (`ticker.renko()`)

Renko charts stack bricks in adjacent columns based on price changes, ignoring time intervals and volume. 

The `ticker.renko()` function generates a ticker identifier to query Renko coordinate ranges, but Pine Script cannot natively plot Renko brick visuals on the chart:
```pinescript
//@version=6
indicator("Renko Lows Request", "", true)

// "ATR" calculation method, ATR period of 10
renkoTicker = ticker.renko(syminfo.tickerid, "ATR", 10)
renkoLow = request.security(renkoTicker, timeframe.period, low)

plot(renkoLow)
```

---

## Line Break (`ticker.linebreak()`)

Line Break charts show vertical boxes constructed based on consecutive closing price comparisons. 

```pinescript
//@version=6
indicator("Line Break Close Request", "", true)

// 3-line break configuration
lineBreakTicker = ticker.linebreak(syminfo.tickerid, 3)
lineBreakClose = request.security(lineBreakTicker, timeframe.period, close)

plot(lineBreakClose)
```

---

## Kagi (`ticker.kagi()`)

Kagi charts plot continuous lines that reverse direction when price movements exceed a predetermined threshold.

```pinescript
//@version=6
indicator("Kagi Close Request", "", true)

// Reversal amount set to 3
kagiBreakTicker = ticker.linebreak(syminfo.tickerid, 3)
kagiBreakClose = request.security(kagiBreakTicker, timeframe.period, close)

plot(kagiBreakClose)
```

---

## Point and Figure (`ticker.pointfigure()`)

Point and Figure (PnF) charts track price movements vertically using columns of X's (price rises) and O's (price falls), ignoring time. Columns are represented programmatically with synthetic open and close prices:

```pinescript
//@version=6
indicator("PnF Close Request", "", true)

// Point & Figure configuration using high/low source, ATR box size method, ATR length 14, reversal amount 3
pnfTicker = ticker.pointfigure(syminfo.tickerid, "hl", "ATR", 14, 3)
[pnfO, pnfC] = request.security(pnfTicker, timeframe.period, [open, close], barmerge.gaps_on)

plot(pnfO, "PnF Open", color.green, 4, plot.style_linebr)
plot(pnfC, "PnF Close", color.red, 4, plot.style_linebr)
```
