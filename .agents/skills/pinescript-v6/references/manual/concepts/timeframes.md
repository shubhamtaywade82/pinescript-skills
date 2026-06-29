# Timeframes

The timeframe of a chart (also called interval or resolution) is the unit of time represented by one bar on the chart. Timeframes are expressed in Pine Script® as string specifications.

---

## Timeframe String Specifications

Timeframe strings are composed of a multiplier and a unit. If the multiplier is omitted, it defaults to `1`.

### Units
*   **Minutes**: No unit letter is specified. E.g. `"1"` (1 minute), `"30"` (30 minutes), `"60"` (1 hour / 60 minutes), `"1440"` (24 hours / 1440 minutes).
*   **Ticks**: `"T"`. E.g., `"1T"`, `"10T"`. (Only multipliers `1`, `10`, `100`, `1000` are valid).
*   **Seconds**: `"S"`. E.g., `"5S"`, `"30S"`. (Only multipliers `1`, `5`, `10`, `15`, `30`, `45` are valid).
*   **Days**: `"D"`. E.g., `"1D"`, `"3D"`.
*   **Weeks**: `"W"`. E.g., `"1W"`, `"2W"`.
*   **Months**: `"M"`. E.g., `"1M"`, `"3M"`.

> [!WARNING]
> There is no hour unit (such as `"1H"`). One hour must be written as `"60"` (sixty minutes), and two hours as `"120"`.

---

## Comparing Timeframes

You can convert timeframe strings to a numerical representation in seconds using `timeframe.in_seconds()` to easily compare resolutions:

```pinescript
//@version=6
indicator("Timeframe comparison example", "", true)

// Allow user to select a timeframe
string tfInput = input.timeframe(defval = "240", title = "Comparison Timeframe")

// Get seconds for both chart resolution and chosen resolution
float chartTFInMinutes = timeframe.in_seconds() / 60
float inputTFInMinutes = timeframe.in_seconds(tfInput) / 60

// Ensure compare timeframe is equal to or higher than chart timeframe
if chartTFInMinutes > inputTFInMinutes
    runtime.error("The chart's timeframe must not be higher than the input's timeframe.")

var table t = table.new(position.top_right, 1, 1)
string txt = "Chart: " + str.tostring(chartTFInMinutes, "#.## min") + 
             "\nInput: " + str.tostring(inputTFInMinutes, "#.## min")

if barstate.isfirst
    table.cell(t, 0, 0, txt, bgcolor = color.yellow)
else if barstate.islast
    table.cell_set_text(t, 0, 0, txt)
```

---

## Timeframe Built-in Variables

*   **`timeframe.period`**: Returns the timeframe string specification of the current chart (e.g. `"60"` or `"1D"`).
*   **`timeframe.multiplier`**: Returns the multiplier of the current chart's timeframe as an `int` (e.g. `5` on a 5-minute chart).
*   **`timeframe.isintraday`**: Returns `true` if the current timeframe is in minutes, seconds, or ticks.
*   **`timeframe.isseconds`**: Returns `true` if the timeframe is in seconds.
*   **`timeframe.isticks`**: Returns `true` if the timeframe is in ticks.
*   **`timeframe.isdaily`**: Returns `true` if the timeframe is in days.
*   **`timeframe.isweekly`**: Returns `true` if the timeframe is in weeks.
*   **`timeframe.ismonthly`**: Returns `true` if the timeframe is in months.
*   **`timeframe.isminutes`**: Returns `true` if the timeframe is in minutes.
