# Conditional Structures

Pine Script® supports conditional branching using `if` and `switch` expressions. Both structures return a value, making them expressions as well.

### `if` Expression
```pinescript
//@version=6
indicator("Conditional structures - if")
float band = if close > ta.sma(close, 20)
    high
else
    low
plot(band)
```

### `switch` Expression
```pinescript
//@version=6
indicator("Conditional structures - switch")
enum Trend
    UP
    DOWN
    SIDEWAYS

Trend currentTrend = Trend.SIDEWAYS
color trendColor = switch currentTrend
    Trend.UP => color.green
    Trend.DOWN => color.red
    => color.gray

plot(close, color=trendColor)
```

See also:
- [Loops](loops.md)
