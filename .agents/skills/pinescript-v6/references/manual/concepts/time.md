# Time

Working with dates and times in Pine Script® involves understanding UNIX timestamps, exchange time zones, chart time zones, and the tools available to format or calculate time differences.

---

## Time Definitions

### 1. UNIX Timestamp
A UNIX timestamp in Pine represents the absolute number of milliseconds elapsed since midnight UTC on January 1, 1970 (the UNIX Epoch). UNIX timestamps are **time zone-agnostic**—a specific timestamp points to the exact same moment in time globally.

### 2. Exchange Time Zone
The time zone of the symbol's exchange, accessed via `syminfo.timezone`. Calendar variables (like `hour`, `dayofweek`, `month`) and time functions use this time zone by default.

### 3. Chart Time Zone
A visual preference set in the chart properties. It alters how time is displayed on the chart axis and in Pine Log prefixes. **Pine scripts cannot access this setting**; calculations always refer to exchange time unless custom timezone arguments are specified.

---

## Time Zones and Strings

Functions that calculate or format time accept timezone arguments in two formats:
*   **UTC/GMT Offset Notation**: E.g. `"UTC-5"`, `"GMT+05:30"`, `"UTC+10"`.
*   **IANA Database Notation**: E.g. `"America/New_York"`, `"Asia/Kolkata"`, `"Australia/Sydney"`.

> [!TIP]
> Always prefer IANA identifiers (e.g. `"America/New_York"`) over fixed UTC offsets. IANA zones automatically adjust calculations for Daylight Saving Time (DST) changes, whereas fixed offsets (e.g., `"UTC-5"`) do not.

```pinescript
//@version=6
indicator("UTC vs IANA time zone strings demo")

// Fixed offset does not adjust for DST:
int hourUTC = hour(time, "UTC-4")

// IANA identifier adjusts automatically:
int hourIANA = hour(time, "America/New_York")

bgcolor(hourUTC != hourIANA ? color.new(color.blue, 80) : na)
```

---

## Time Variables Reference

*   **`time`**: The UNIX opening timestamp of the current bar.
*   **`time_close`**: The UNIX closing timestamp of the current bar (on time-based charts).
*   **`time_tradingday`**: The UNIX timestamp representing the beginning (00:00 UTC) of the trading day. (Useful for symbols with sessions that cross midnight).
*   **`timenow`**: The UNIX timestamp of the current system clock time when the script executes. 
*   **Calendar Variables**: `year`, `month`, `weekofyear`, `dayofmonth`, `dayofweek`, `hour`, `minute`, `second`. Represent fields for the current bar's open time, in the exchange time zone.
*   **`last_bar_time`**: The UNIX opening timestamp of the last available bar on the chart.
*   **`chart.left_visible_bar_time`** / **`chart.right_visible_bar_time`**: UNIX opening timestamps of the leftmost and rightmost visible bars.

---

## Key Time Functions

### 1. `time()` and `time_close()`
Return UNIX timestamps for bars on any timeframe. Can filter by session:
```pinescript
// Returns bar open time if it falls inside 11:00 to 13:00, otherwise returns na
bool inSession = not na(time(timeframe.period, "1100-1300"))
```
*   **Offsets**: Use `bars_back` and `timeframe_bars_back` to retrieve historical or future timestamps without using `[]` history operators.
    ```pinescript
    // Get opening time of the bar 2 periods back on a daily timeframe
    int openTime = time("1D", bars_back = 2)
    ```

### 2. `timestamp()`
Calculates a UNIX timestamp from calendar components:
```pinescript
// Using calendar arguments (defaults to exchange time zone)
int ts1 = timestamp(2024, 10, 31, 0, 0, 0)

// Using IANA zone prefix
int ts2 = timestamp("America/New_York", 2024, 10, 31, 0, 0, 0)

// Using a date string (defaults to GMT/UTC+0)
int ts3 = timestamp("31 Oct 2024 00:00:00 UTC")
```

### 3. `str.format_time()`
Formats a UNIX timestamp into a readable text string:
```pinescript
str.format_time(time, format, timezone)
```
Default format is ISO 8601: `"yyyy-MM-dd'T'HH:mm:ssZ"`.

| Token | Represents | Example |
| :--- | :--- | :--- |
| `"yyyy"` | Year | `"2024"` |
| `"MMMM"` | Full Month Name | `"October"` |
| `"dd"` | Two-digit Day | `"31"` |
| `"HH"` | 24-Hour | `"15"` |
| `"hh"` | 12-Hour | `"03"` |
| `"mm"` | Minute | `"45"` |
| `"a"` | AM/PM | `"PM"` |
| `"zzzz"` | Full Time Zone Name | `"Eastern Standard Time"` |
| `"'Text'"` | Escaped literal text | `"Bar expected closing time"` |

---

## Expressing Time Differences

The difference between two UNIX timestamps represents a span of milliseconds. **Do not pass this duration directly into `str.format_time()`**, as the function will interpret the duration as an elapsed time from the epoch (1970-01-01), causing incorrect dates.

Instead, calculate units mathematically:

```pinescript
//@version=6
indicator("Calculating time span demo")

// Duration constants
const int ONE_DAY    = 86400000 
const int ONE_HOUR   = 3600000
const int ONE_MINUTE = 60000

int startTime = input.time(timestamp("1 Jan 2024 00:00 UTC"))
int endTime   = input.time(timestamp("2 Jan 2024 12:30 UTC"))

// Calculate difference
int diff = math.abs(endTime - startTime)

int days = math.floor(diff / ONE_DAY)
diff %= ONE_DAY

int hours = math.floor(diff / ONE_HOUR)
diff %= ONE_HOUR

int minutes = math.floor(diff / ONE_MINUTE)

// Output formatted result
string result = str.format("{0} days, {1} hours, {2} minutes", days, hours, minutes)
```
