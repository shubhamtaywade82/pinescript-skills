# Chart Information

Pine Script® provides a subset of built-in variables that allow your scripts to retrieve metadata about the active chart, prices, symbol, timeframe, and trading session.

---

## Prices and Volume

The primary built-in time series variables for OHLCV data are:
*   **`open`**: The bar’s opening price.
*   **`high`**: The bar’s highest price, or the highest price reached during the developing realtime bar.
*   **`low`**: The bar’s lowest price, or the lowest price reached during the developing realtime bar.
*   **`close`**: The bar’s closing price, or the current price in the developing realtime bar.
*   **`volume`**: The volume traded during the bar. The unit depends on the asset type (e.g. shares for stocks, contracts for futures, base currency units for crypto, etc.).

Additionally, average indicators are available:
*   **`hl2`**: Average of high and low (`(high + low) / 2`).
*   **`hlc3`**: Average of high, low, and close (`(high + low + close) / 3`).
*   **`ohlc4`**: Average of open, high, low, and close (`(open + high + low + close) / 4`).

### Historical vs. Realtime Bars
*   On **historical bars**, values are fixed. The script executes once on the bar close when all information is finalized.
*   On **realtime bars**, price and volume variables (except `open`) fluctuate continuously on every tick update. This can lead to repainting indicators.

Past values of price series can be accessed using the `[]` history-referencing operator (e.g. `close[1]` represents the close of the previous bar).

---

## Symbol Information

Built-in variables in the `syminfo` namespace provide details about the chart’s symbol. If the symbol changes, the script re-executes using the updated metadata:

*   **`syminfo.basecurrency`**: The base currency of the pair (e.g., "BTC" in BTCUSD).
*   **`syminfo.currency`**: The quote currency of the pair (e.g., "USD" in BTCUSD).
*   **`syminfo.description`**: The long description of the symbol.
*   **`syminfo.main_tickerid`**: The main ticker identifier of the chart (e.g. `NASDAQ:AAPL`). Unlike `syminfo.tickerid`, this variable always represents the main chart's ticker ID, even when evaluated inside a `request.security()` call.
*   **`syminfo.mincontract`**: The minimum tradable amount set by the exchange (e.g., `1` for shares, `0.0001` for some crypto).
*   **`syminfo.mintick`**: The tick value (minimum increment the price can move).
*   **`syminfo.pointvalue`**: The multiplier determining contract value (useful in futures pricing).
*   **`syminfo.prefix`**: The exchange or broker prefix (e.g., `NASDAQ`, `BINANCE`, `CME_MINI_DL`).
*   **`syminfo.root`**: The root prefix for structured futures symbols (e.g., "ES" for ES1!, "ZW" for ZW1!).
*   **`syminfo.session`**: The session type (e.g. "regular" or "extended").
*   **`syminfo.ticker`**: The symbol name without the exchange prefix (e.g. `BTCUSD`, `AAPL`).
*   **`syminfo.tickerid`**: Consists of the exchange prefix and symbol name (e.g. `NASDAQ:MSFT`). When used inside `request.security()`, it dynamically points to the requested context's ticker.
*   **`syminfo.timezone`**: The timezone the symbol is traded in (e.g., "America/New_York").
*   **`syminfo.type`**: The market sector. Possible values: `stock`, `futures`, `index`, `forex`, `crypto`, `fund`, `dr`, `cfd`, `bond`, `warrant`, `structured`, `right`.

### Symbol Metadata Table Example
This script displays all symbol metadata inside a chart table:

```pinescript
//@version=6
indicator("`syminfo.*` built-ins demo", overlay = true)

string txtLeft =
  "syminfo.basecurrency: "  + "\n" +
  "syminfo.currency: "      + "\n" +
  "syminfo.description: "   + "\n" +
  "syminfo.main_tickerid: " + "\n" +
  "syminfo.mincontract: "   + "\n" +
  "syminfo.mintick: "       + "\n" +
  "syminfo.pointvalue: "    + "\n" +
  "syminfo.prefix: "        + "\n" +
  "syminfo.root: "          + "\n" +
  "syminfo.session: "       + "\n" +
  "syminfo.ticker: "        + "\n" +
  "syminfo.tickerid: "      + "\n" +
  "syminfo.timezone: "      + "\n" +
  "syminfo.type: "

string txtRight =
  syminfo.basecurrency              + "\n" +
  syminfo.currency                  + "\n" +
  syminfo.description               + "\n" +
  syminfo.main_tickerid             + "\n" +
  str.tostring(syminfo.mincontract) + "\n" +
  str.tostring(syminfo.mintick)     + "\n" +
  str.tostring(syminfo.pointvalue)  + "\n" +
  syminfo.prefix                    + "\n" +
  syminfo.root                      + "\n" +
  syminfo.session                   + "\n" +
  syminfo.ticker                    + "\n" +
  syminfo.tickerid                  + "\n" +
  syminfo.timezone                  + "\n" +
  syminfo.type

if barstate.islast
    var table t = table.new(position.middle_right, 2, 1)
    table.cell(t, 0, 0, txtLeft, bgcolor = color.yellow, text_halign = text.align_right)
    table.cell(t, 1, 0, txtRight, bgcolor = color.yellow, text_halign = text.align_left)
```

---

## Chart Timeframe

You can identify the chart's timeframe category using these boolean variables:
*   `timeframe.isseconds`
*   `timeframe.isminutes`
*   `timeframe.isintraday`
*   `timeframe.isdaily`
*   `timeframe.isweekly`
*   `timeframe.ismonthly`
*   `timeframe.isdwm` (True if the timeframe is daily, weekly, or monthly).

### Multipliers and Periods
*   **`timeframe.multiplier`** (`simple int`): Returns the integer multiplier (e.g. `60` on a 1-hour chart, `30` on a 30-second chart, `3` on a quarterly chart).
*   **`timeframe.period`** (`string`): Represents the timeframe as a standard Pine timeframe string (e.g. `1D`, `2W`, `15`). Dynamically points to the requested context's timeframe when evaluated inside `request.security()`.
*   **`timeframe.main_period`** (`string`): Always returns the main chart's timeframe, even when evaluated inside requested security contexts.

---

## Session Information

*   **`syminfo.session`**: Returns either `session.regular` or `session.extended` depending on symbol configuration and chart settings.
*   **Session states**: Specific variables (e.g. `session.ispremarket`, `session.ismarket`, `session.ispostmarket`) help scripts determine the session state of the current bar.
