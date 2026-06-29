# Other Timeframes and Data

Pine Script® allows scripts to request data from symbols, resolutions, or contexts other than those utilized by the current chart. The `request.*()` family of functions facilitates fetching information from various alternative sources:

*   `request.security()`: Retrieves data from another symbol, timeframe, or modified context.
*   `request.security_lower_tf()`: Retrieves intrabar data (an array of values from a timeframe lower than the chart resolution).
*   `request.currency_rate()`: Requests a conversion rate to translate values from one currency to another.
*   `request.dividends()`, `request.splits()`, and `request.earnings()`: Retrieve stock corporate action metrics on exact dates.
*   `request.financial()`: Retrieves FactSet financial data (income statement, balance sheet, cash flows, etc.).
*   `request.economic()`: Retrieves macroeconomic metrics by country or region code.
*   `request.footprint()`: Retrieves volume footprint data (Premium/Ultimate plans only).
*   `request.seed()`: Retrieves custom EOD data feeds stored in user-maintained GitHub repositories.

---

## Common Characteristics

Many `request.*()` functions share a set of parameters and behaviors:

### 1. Behavior and Limits
Each unique `request.*()` call in a script executes a distinct network/database fetch. 
*   **Request Limit**: Scripts can make up to **40 unique requests** (or up to **64** on the Ultimate plan).
*   Redundant calls with identical arguments share a cache and count as a single unique request.

### 2. The `gaps` Parameter
The `gaps` parameter determines how the script handles empty data slots when transferring higher-timeframe data to a lower-timeframe chart:
*   `barmerge.gaps_off` (Default): Fills gaps using forward-filling. The function returns the last known value until a new bar is confirmed.
*   `barmerge.gaps_on`: Returns `na` values on bars where a new timeframe candle has not yet finalized.

```pinescript
//@version=6
indicator("Gaps Demo", overlay = true)

// gaps_off (Default)
closeGapsOff = request.security(syminfo.tickerid, "60", close, gaps = barmerge.gaps_off)
// gaps_on (Leaves breaks)
closeGapsOn  = request.security(syminfo.tickerid, "60", close, gaps = barmerge.gaps_on)

plot(closeGapsOff, "Gaps Off", color = color.blue)
plot(closeGapsOn,  "Gaps On",  color = color.red, style = plot.style_linebr)
```

### 3. The `ignore_invalid_symbol` Parameter
If `ignore_invalid_symbol = false` (default), a nonexistent ticker ID causes a runtime crash. Setting it to `true` returns `na` values instead of halting execution.

### 4. The `currency` Parameter
Converts currency-denominated data (prices, financials) using daily rates from major exchanges:
```pinescript
// Fetches MSFT close converted to Japanese Yen
msftInJPY = request.security("NASDAQ:MSFT", "D", close, currency = currency.JPY)
```

### 5. The `lookahead` Parameter
Controls lookahead bias on historical bars:
*   `barmerge.lookahead_off` (Default): HTF values are returned only at the *close* of the HTF bar.
*   `barmerge.lookahead_on`: HTF values are returned at the *open* of the HTF bar. On historical bars, this leaks future values into the past (lookahead bias) unless the expression argument includes a historical offset (e.g., `close[1]`).

---

## Dynamic Requests (Pine Script v6)

In Pine Script v6, requests are **dynamic by default**. Unlike previous versions, v6 scripts can:
1.  Use `series` variables as the `symbol` or `timeframe` arguments (allowing symbols to change programmatically during execution).
2.  Execute requests inside conditional blocks (`if`, `switch`), loops (`for`), and exported library functions.
3.  Perform **nested requests** where one request evaluations triggers another.

> [!WARNING]
> While arguments can be dynamic `series` strings, all required datasets must be queried while executing on **historical bars**. Scripts cannot request a brand new symbol/timeframe context on realtime bars.

### Conditional Request Inside a Loop
```pinescript
//@version=6
indicator("Dynamic Loop Requests", overlay = false)

// Declaring list of tickers
var symbols = array.from("NASDAQ:AAPL", "NASDAQ:MSFT", "NASDAQ:GOOG")
var requestedData = array.new<float>()

if barstate.islast
    requestedData.clear()
    for symbol in symbols
        // Execute request.security inside the loop block
        float vol = request.security(symbol, timeframe.period, volume)
        requestedData.push(vol)
```

### Nested Requests
Nested requests inherit the symbol/timeframe context of the wrapping function:
```pinescript
//@version=6
indicator("Nested Requests", "", true)

// Evaluating custom ticker and timeframe info
infoExpression = syminfo.tickerid + ":" + timeframe.period

// Inner request gets evaluated inside the outer request's context
requestedInfo = request.security("NASDAQ:AAPL", "60", infoExpression)
```

---

## `request.security()`

Retrieves custom calculations from other symbol/timeframe contexts.

### HTF Request Without Repainting (Recommended Pattern)
To avoid repainting, shift the requested series by one bar and use `lookahead = barmerge.lookahead_on`:
```pinescript
//@version=6
indicator("Non-Repainting HTF MA", overlay = true)

string htf = input.timeframe("1D", "Timeframe")

// Avoid repainting: offset expression by [1] and enable lookahead
float htfMA = request.security(syminfo.tickerid, htf, ta.sma(close, 20)[1], lookahead = barmerge.lookahead_on)

plot(htfMA, "HTF SMA", color = color.aqua)
```

### Requesting Custom Chart Contexts
Using `ticker.*()` helper functions, you can request custom adjusted ticker IDs:
*   `ticker.new()`: Custom prefix, symbol, session, and adjustment parameters.
*   `ticker.heikinashi()`: Request synthetic Heikin-Ashi candlestick data.
*   `ticker.renko()`, `ticker.pointfigure()`, `ticker.kagi()`, `ticker.linebreak()`: Fetch coordinates from non-standard charts.
*   `ticker.inherit()`: Copies modifiers from one ticker to another.

```pinescript
//@version=6
indicator("Heikin-Ashi Request", overlay = true)

// Construct Heikin-Ashi ticker
haTicker = ticker.heikinashi(syminfo.tickerid)
haClose = request.security(haTicker, timeframe.period, close)

plot(haClose, "Heikin-Ashi Close", color = color.purple, linewidth = 2)
```

---

## `request.security_lower_tf()`

Retrieves intrabar data from resolutions lower than or equal to the chart timeframe. Because multiple lower-timeframe bars exist within a single chart candle, this function returns an **array** containing all corresponding values sorted chronologically.

```pinescript
//@version=6
indicator("Lower Timeframe Decomposer")

// Retrieve 1-minute closes on a higher-timeframe chart
array<float> ltfCloses = request.security_lower_tf(syminfo.tickerid, "1", close)

// Calculate sum of intrabar closes
float sumCloses = 0.0
if array.size(ltfCloses) > 0
    for val in ltfCloses
        sumCloses += val

plot(sumCloses / math.max(1, array.size(ltfCloses)), "Average LTF Close")
```

---

## Macro & Fundamental Data Triggers

### 1. `request.currency_rate()`
Fetches conversion rates without manually specifying symbol spreads:
```pinescript
// Direct conversion from Turkish Lira (TRY) to South Korean Won (KRW)
rate = request.currency_rate("TRY", "KRW")
```

### 2. `request.dividends()`, `request.splits()`, and `request.earnings()`
Fetches specific corporate action metrics on exact dates:
```pinescript
//@version=6
indicator("Earnings and Dividends Info", overlay = true)

// EPS info
epsActual   = request.earnings(syminfo.tickerid, earnings.actual, gaps = barmerge.gaps_on)
epsEstimate = request.earnings(syminfo.tickerid, earnings.estimate, gaps = barmerge.gaps_on)

// Payouts
divGross = request.dividends(syminfo.tickerid, dividends.gross, gaps = barmerge.gaps_on)
```

### 3. `request.financial()`
Retrieves annual (`"FY"`), quarterly (`"FQ"`), semiannual (`"FH"`), or trailing twelve months (`"TTM"`) financial indicators from FactSet:
```pinescript
//@version=6
indicator("Financials Demo")

// Fetch Cost of Goods Sold quarterly
cogs = request.financial(syminfo.tickerid, "COST_OF_GOODS", "FQ")
plot(cogs, "COGS")
```

### 4. `request.economic()`
Requests global macroeconomic data from regions (e.g. GDP, inflation, unemployment rate):
```pinescript
//@version=6
indicator("US GDP Growth")
usGdp = request.economic("US", "GDPQQ")
plot(usGdp, "US GDP growth rate")
```

---

## Volume Footprint (`request.footprint()`)

*Exclusive to Premium and Ultimate plans.* Volume footprints detail buy/sell volume distributions across price levels inside a single bar.

```pinescript
//@version=6
indicator("Volume Footprint Delta")

// ticks_per_row sets vertical bin sizing (e.g. 100 ticks = 100 * mintick)
var footprint fp = request.footprint(100)

// Retrieve delta values
float delta = na(fp) ? na : footprint.delta(fp)

plot(delta, "Footprint Delta", color = delta > 0 ? color.green : color.red, style = plot.style_columns)
```
