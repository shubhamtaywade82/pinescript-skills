# Sessions

Exchanges define a trading session for every symbol representing the times of day and days of the week in which the symbol can be traded. Subsessions (pre-market, post-market, electronic trading hours) can also be defined. 

Programmers can use session-related Pine Script® variables and functions to check whether bars fall within specified time periods, query data from alternative sessions, or track market states.

---

## Time-Based Sessions

A custom session is defined by encoding the start time, end time, and days of the week into a session string.

### Syntax
```text
<time_period>:<days>
```
*   **`<time_period>`**: Represents start and end times in `"HHmm-HHmm"` 24-hour format (e.g., `"0930-1600"`). Multiple periods can be comma-separated (e.g. `"0900-1200,1300-1700"`).
*   **`<days>`**: Represents days of the week where `1` is Sunday, `2` is Monday, through `7` for Saturday. If omitted, the session applies to all days (`1234567`).
*   **`"24x7"`**: A special session string format equivalent to `"0000-0000:1234567"` (24 hours, 7 days a week).

### Standard Examples

| Session String | Description |
| :--- | :--- |
| `"0000-0000"` | 24-hour session, 7 days a week. |
| `"0000-0000:23456"` | 24-hour session, Monday through Friday. |
| `"2000-1630:1234567"` | Overnight session from 8:00 PM to 4:30 PM the next day. |
| `"0930-1700:146"` | 9:30 AM to 5:00 PM on Sundays (1), Wednesdays (4), and Fridays (6). |

### Using Time-Based Sessions
The `time()` and `time_close()` functions accept session strings. They return the bar's UNIX timestamp if the bar falls inside the session, and `na` if it falls outside:

```pinescript
//@version=6
indicator("Session bar checker", overlay = true)

string sessionInput = input.session("0900-1130", "Session")

// Check if open/close timestamps fall within the session
bool isBarOpenInSession  = not na(time("", sessionInput))
bool isBarCloseInSession = not na(time_close("", sessionInput))

if isBarOpenInSession
    label.new(bar_index, high, "Open in Session: " + str.format_time(time, "HH:mm"), color = color.green)
```

---

## Named Sessions

Exchanges often define standard subsessions such as regular sessions or extended electronic sessions.

To identify or query named sessions:
*   **`syminfo.session`**: Returns the active session name on the current chart (e.g., `"regular"` or `"extended"`).
*   **`session.regular`**: Built-in string constant representing regular hours.
*   **`session.extended`**: Built-in string constant representing extended hours.

> [!IMPORTANT]
> Futures markets often treat the longer Electronic Trading Hours (ETH) session as `"regular"`. On these symbols, the US Pit session is designated as `"us_regular"`.

---

## Creating Session-Specific Tickers

You can query data from alternative sessions by creating custom ticker IDs using `ticker.new()` or `ticker.modify()` and querying them via `request.security()`.

```pinescript
//@version=6
indicator("Visualizing session-specific data", overlay = true)

// Create ticker IDs specifying alternative session contexts
string extendedTicker = ticker.new(syminfo.prefix, syminfo.ticker, session.extended)
string regularTicker  = ticker.new(syminfo.prefix, syminfo.ticker, session.regular)

// Fetch close prices from regular and extended sessions
float extendedClose = request.security(extendedTicker, timeframe.period, close, gaps = barmerge.gaps_on)
float regularClose  = request.security(regularTicker, timeframe.period, close, gaps = barmerge.gaps_on)

// Plot line breaks over gaps
plot(extendedClose, "Extended Close", color = color.black, style = plot.style_linebr)
plot(regularClose,  "Regular Close",  color = color.red,   style = plot.style_circles)
```

---

## Session Variables Reference

### Market States

*   **`session.ismarket`**: True when the current bar is within regular trading hours. (Always true on daily timeframe charts and higher).
*   **`session.ispremarket`**: True during the extended trading hours preceding the regular session. (Always false on daily charts and higher).
*   **`session.ispostmarket`**: True during the extended trading hours following the regular session. (Always false on daily charts and higher).

### First and Last Bars

*   **`session.isfirstbar`**: True on the first bar of the day's session (including pre-market bars if extended session data is enabled).
*   **`session.isfirstbar_regular`**: True on the first bar of the regular trading session.
*   **`session.islastbar`**: True on the last bar of the day's session (including post-market bars if extended session data is enabled).
*   **`session.islastbar_regular`**: True on the last bar of the regular trading session.

> [!WARNING]
> If no price ticks occur during the duration of the final bar, `session.islastbar` and `session.islastbar_regular` may not evaluate to true on any bar. In contrast, first bar flags will always trigger.
