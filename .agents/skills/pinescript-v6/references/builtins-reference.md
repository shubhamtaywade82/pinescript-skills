# Pine Script® v6 Language Reference Summary

This file summarizes TradingView's official v6 reference material and organizes it for the agent skill.

## 1. Type Keywords
- **array** — Created via `array.new<type>()` or `array.from()`. Array objects (or IDs) are always of `"series"` form.
- **bool** — Boolean variables have values `true` or `false`. Explicit declaration is optional.
- **box** — Box IDs are created via `box.new()`. Box objects are always of `"series"` form.
- **chart.point** — Chart point instances are produced via `chart.point.from_time()`, `chart.point.from_index()`, `chart.point.now()`, and `chart.point.new()`.
  - `index` — `series int` — x-coordinate as bar index.
  - `time` — `series int` — x-coordinate as UNIX time in milliseconds.
  - `price` — `series float` — y-coordinate.
- **color** — Color literals use `#RRGGBB` or `#RRGGBBAA`; `AA` defaults to `FF` when omitted.
- **const** — Assigns the `"const"` type qualifier to variables and parameters of non-exported functions. Values are established at compile time and never change.
- **float** — Floating-point type. Explicit declaration is optional except when initialized with `na`.
- **footprint** — Objects are created via `request.footprint()`. Used with `footprint.*()` functions to retrieve volume footprint data.
- **int** — Integer type. Explicit declaration is optional except when initialized with `na`.
- **label** — Label IDs are created via `label.new()`. Label objects are always of `"series"` form.
- **line** — Line IDs are created via `line.new()`. Line objects are always of `"series"` form.
- **linefill** — Linefill objects are created via `linefill.new()`. Linefill objects are always of `"series"` form.
- **map** — Map objects are created via `map.new<type,type>()` and are always of `series` form.
- **matrix** — Matrix objects are created via `matrix.new<type>()` and are always of `"series"` form.
- **polyline** — Polyline instances are produced via `polyline.new()`.
- **series** — Assigns the `"series"` type qualifier to variables and function parameters. Values can change throughout a script's execution.
- **simple** — Assigns the `"simple"` type qualifier to variables and function parameters. Values do not change across bars.
- **string** — String type. Explicit declaration is optional except when initialized with `na`.
- **table** — Table objects are created via `table.new()`. Table objects are always of `"series"` form.
- **volume_row** — Declares variables or parameters as `volume_row`.

## 2. Qualifier Hierarchy
- Weakest to strongest: `const` < `input` < `simple` < `series`

## 1. Indicator and Strategy Declarations

### `indicator(title, shorttitle, overlay, format, precision, scale, max_bars_back, max_lines_count, max_labels_count, max_boxes_count, max_polylines_count)`
Declares a new indicator (study) on the chart.
- `overlay`: If `true`, the indicator is plotted directly on the main price chart. If `false`, it displays in a separate pane.

### `strategy(title, shorttitle, overlay, initial_capital, default_qty_type, default_qty_value, currency, margin_long, margin_short, commission_type, commission_value)`
Declares a new strategy script (used for backtesting and automation).
- In v6, the default value for `margin_long` and `margin_short` is **100%**.

---

## 2. Technical Analysis (`ta.*`)

The `ta` namespace provides core mathematical indicators and analysis tools:

| Function | Signature | Description |
| :--- | :--- | :--- |
| **`ta.sma`** | `ta.sma(source, length)` | Simple Moving Average. |
| **`ta.ema`** | `ta.ema(source, length)` | Exponential Moving Average. |
| **`ta.rma`** | `ta.rma(source, length)` | Moving Average used in RSI and ATR. |
| **`ta.wma`** | `ta.wma(source, length)` | Weighted Moving Average. |
| **`ta.vwap`** | `ta.vwap(source)` | Volume Weighted Average Price. |
| **`ta.rsi`** | `ta.rsi(source, length)` | Relative Strength Index. |
| **`ta.macd`** | `ta.macd(source, fast, slow, signal)` | Moving Average Convergence Divergence. Returns `[macdLine, signalLine, histLine]`. |
| **`ta.atr`** | `ta.atr(length)` | Average True Range. |
| **`ta.highest`** | `ta.highest(source, length)` | Highest value in the range. |
| **`ta.lowest`** | `ta.lowest(source, length)` | Lowest value in the range. |
| **`ta.crossover`** | `ta.crossover(source1, source2)` | Returns `true` if `source1` crossed over `source2`. |
| **`ta.crossunder`** | `ta.crossunder(source1, source2)` | Returns `true` if `source1` crossed under `source2`. |
| **`ta.pivothigh`** | `ta.pivothigh(source, left, right)` | Price index/value of a pivot high. |
| **`ta.pivotlow`** | `ta.pivotlow(source, left, right)` | Price index/value of a pivot low. |
| **`ta.change`** | `ta.change(source, length)` | Difference between current value and previous value (`source - source[length]`). |

---

## 3. Input Types (`input.*`)

Used to create user settings in the "Inputs" panel.

- **`input.bool(defval, title, tooltip, inline, group, active)`**
- **`input.int(defval, title, minval, maxval, step, tooltip, inline, group, active)`**
- **`input.float(defval, title, minval, maxval, step, tooltip, inline, group, active)`**
- **`input.string(defval, title, options, tooltip, inline, group, active)`**
- **`input.symbol(defval, title, tooltip, inline, group, active)`**
- **`input.timeframe(defval, title, options, tooltip, inline, group, active)`**
- **`input.source(defval, title, tooltip, inline, group, active)`** (e.g., `close`, `open`, `hl2`)
- **`input.color(defval, title, tooltip, inline, group, active)`**
- **`input.enum(defval, title, tooltip, inline, group, active)`**: (v6 Specific) Creates a dropdown menu for a user-defined `enum`.

---

## 4. Market and Environment Information

### Symbol Information (`syminfo.*`)
- `syminfo.ticker`: Ticker name (e.g., `"AAPL"`).
- `syminfo.tickerid`: Exchange + ticker name (e.g., `"NASDAQ:AAPL"`).
- `syminfo.mintick`: Minimum price increment (e.g., `0.01`).
- `syminfo.pointvalue`: Currency multiplier per point.
- `syminfo.currency`: Currency symbol code (e.g., `"USD"`).
- `syminfo.mincontract`: (v6 Specific) Minimum contract size.

### Timeframes (`timeframe.*`)
- `timeframe.period`: Current chart timeframe string (e.g., `"D"`, `"240"`, `"60"`).
- `timeframe.isminutes`: `true` if current timeframe is in minutes.
- `timeframe.isdaily`: `true` if daily.

### Time variables (`time` / `time_close` / `timenow`)
- `time`: UNIX time of the current bar open (in milliseconds).
- `time_close`: UNIX time of the current bar close.
- `timenow`: UNIX time of current system time.

---

## 5. Mathematical Operations (`math.*`)

| Function | Description |
| :--- | :--- |
| `math.abs(x)` | Absolute value of `x`. |
| `math.ceil(x)` | Smallest integer greater than or equal to `x`. |
| `math.floor(x)` | Largest integer less than or equal to `x`. |
| `math.round(x, precision)` | Round `x` to `precision` decimal places. |
| `math.max(x1, x2, ...)` | Maximum value among arguments. |
| `math.min(x1, x2, ...)` | Minimum value among arguments. |
| `math.pow(base, exponent)` | Computes `base^exponent`. |
| `math.sqrt(x)` | Square root of `x`. |
| `math.log(x)` | Natural logarithm of `x`. |
| `math.exp(x)` | Exponential (`e^x`). |

---

## 6. Drawing Objects (`line.*` / `label.*` / `box.*` / `table.*` / `polyline.*`)

Drawings persist across bars. They are referenced using IDs (e.g., `line myLine = line.new(...)`).

### Lines
`line.new(x1, y1, x2, y2, xloc, extend, color, style, width)`
`line.set_xy1(id, x, y)` / `line.set_xy2(id, x, y)`
`line.delete(id)`

### Labels
`label.new(x, y, text, xloc, yloc, color, style, textcolor, size, textalign)`
`label.set_text(id, text)`
`label.delete(id)`

### Boxes
`box.new(left, top, right, bottom, border_color, border_width, border_style, extend, bgcolor)`
`box.delete(id)`

### Tables
`table.new(position, columns, rows, bgcolor, border_color, border_width, frame_color, frame_width)`
`table.cell(table_id, column, row, text, width, height, text_color, text_size, bgcolor)`

### Polylines (v6 Specific)
`polyline.new(points, closed, curved, line_color, fill_color, line_width)`
- Connects an array of `chart.point` objects.
