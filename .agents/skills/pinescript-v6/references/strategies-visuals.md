# Pine Script® v6 Strategies, Visuals, and Inputs

This guide covers changes to strategies, drawing tools (polylines), runtime logging, and settings input parameters introduced in Pine Script® v6.

## 1. Strategy Execution Changes

### Removal of `when` Parameter
The `when` parameter has been removed from all `strategy.*` functions (e.g., `strategy.entry()`, `strategy.close()`, `strategy.exit()`). All order control must be wrapped inside standard `if` blocks.

```pinescript
// ❌ INVALID in v6
strategy.entry("Buy", strategy.long, when = buyCondition)

// ✅ VALID in v6
if buyCondition
    strategy.entry("Buy", strategy.long)
```

### Margin Settings
The default value for `margin_long` and `margin_short` parameters in the `strategy()` declaration statement is now **100%**. This avoids hidden leverage during backtests unless explicitly specified.

### Trade Limit Handling
If a strategy exceeds the limit of **9,000** closed trades, it will no longer crash or fail to execute. Instead, the strategy engine will automatically and silently trim the oldest trades from the beginning of the trade list, keeping the strategy running smoothly.

### Exit Parameter Logic
In `strategy.exit()`, providing both relative parameters (e.g., `profit`, `loss`) and absolute parameters (e.g., `limit`, `stop`) no longer causes the engine to ignore the relative ones. Both can now be specified, and the strategy will exit based on whichever triggers first.

---

## 2. Visuals & Drawings: Polylines

The `polyline` type allows drawing complex multi-point paths and closed, filled shapes using a single object, reducing object limits and performance overhead.

### Creating a Polyline
1. Create an array of `chart.point` objects using `chart.point.new()` or `chart.point.from_index()`.
2. Instantiate the polyline using `polyline.new()`.

```pinescript
//@version=6
indicator("Polyline Demo", overlay = true)

if barstate.islast
    var points = array.new<chart.point>()
    points.push(chart.point.from_index(bar_index - 10, low - 5))
    points.push(chart.point.from_index(bar_index - 5, high + 5))
    points.push(chart.point.from_index(bar_index, close))
    
    polyline.new(points = points, closed = false, line_color = color.blue, line_width = 2)
```

---

## 3. Runtime Logging

Pine Script v6 supports printing to a dedicated **Pine Logs** console.
- **Log Functions**: `log.info()`, `log.warning()`, and `log.error()`.
- **String Formatting**: Supports dynamic placeholders with `{0}`, `{1}`, etc., similar to `str.format()`.

```pinescript
if barstate.islast
    log.info("Finished processing. Current close is {0} and open is {1}", close, open)
```

*Note: Logs are only visible in personal (un-published) scripts.*

---

## 4. Input Configuration Changes

### Conditional Visibility (`active` Parameter)
Every `input.*()` function supports an `active` parameter. Setting it to `false` disables/grays out the input in the Settings dialog, allowing you to build dynamic UIs where certain settings are only accessible if an enabling option is checked.

```pinescript
// Enable custom settings toggle
i_useCustom = input.bool(false, "Use Custom Period")

// Grayed out if 'i_useCustom' is false
i_period    = input.int(14, "Custom Period", active = i_useCustom)
```

### Horizontal Grouping (`inline` Parameter)
Group multiple input variables on a single horizontal row in the settings panel by using the same string name in their `inline` parameter.

```pinescript
i_maColor = input.color(color.blue, "Color", inline = "MA Style")
i_maWidth = input.int(2, "Width", inline = "MA Style")
```
