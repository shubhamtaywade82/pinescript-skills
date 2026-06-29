# Pine Script® v6 Dynamic Data Requests

In Pine Script® v6, the `request.*()` family of functions can be called dynamically. This is a massive upgrade over older versions which required constant strings and limited these calls to the global scope.

## 1. Features of Dynamic Requests

- **Local Scope Execution**: You can invoke `request.security()`, `request.financial()`, `request.dividends()`, etc., inside local scopes, such as `if`/`switch` blocks, `for` loops, and user-defined functions or methods.
- **Series Arguments**: The `symbol` and `timeframe` parameters accept "series string" values instead of just "simple string" or "const string" values. You can construct symbols dynamically at runtime.
- **Strict Limit of 40 Calls**: There is still a limit of **40 unique `request.*()` calls** per script. Every unique combination of symbol, timeframe, and expression evaluates as a separate call. Calling `request.security()` dynamically within a loop on many tickers can easily exceed this limit if not carefully managed.

---

## 2. Dynamic Requests in Action

The following example demonstrates how to iterate over an array of symbols to fetch their closing prices dynamically inside a loop on the last bar.

```pinescript
//@version=6
indicator("Dynamic Security Loop", overlay = true)

// Create array of tickers
var string[] tickers = array.from("AAPL", "MSFT", "GOOG", "TSLA")
var float[] tickerCloses = array.new_float(0)

// Fetch data dynamically on the last bar
if barstate.islast
    array.clear(tickerCloses)
    for ticker in tickers
        // request.security called dynamically inside a loop with a series string
        float tickerClose = request.security(ticker, timeframe.period, close)
        array.push(tickerCloses, tickerClose)
        
    // Log the fetched prices
    for i = 0 to array.size(tickers) - 1
        log.info("Ticker: {0}, Close: {1}", array.get(tickers, i), array.get(tickerCloses, i))
```

---

## 3. Best Practices & Optimization

Dynamic requests are convenient but can be resource-intensive if misused. Follow these optimization guidelines:

### Bundle Multiple Values in Tuples
Instead of requesting open, high, low, and close in four separate calls, request them as a tuple in a single call to save execution time and network overhead:
```pinescript
// ✅ Optimal (1 call)
[o, h, l, c] = request.security(ticker, "D", [open, high, low, close])

// ❌ Inefficient (4 calls)
float o = request.security(ticker, "D", open)
float h = request.security(ticker, "D", high)
float l = request.security(ticker, "D", low)
float c = request.security(ticker, "D", close)
```

### Restrict Loop Runs to the Last Bar
Since `request.security` inside a loop will run on every single historical bar if placed in the global scope, wrap multi-symbol loops in a `barstate.islast` condition when you only need the current real-time metrics.

### Avoid Unnecessary Request Changes
If symbol names are static inputs, assign them globally rather than regenerating strings on every single calculation tick.
