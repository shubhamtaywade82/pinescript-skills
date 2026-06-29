# Limitations

Because each script uses computational resources in the cloud, we must impose limits in order to share these resources fairly among our users. We strive to set as few limits as possible, but will of course have to implement as many as needed for the platform to run smoothly. Limitations apply to the amount of data requested from additional symbols, execution time, memory usage and script size.

If you develop complex scripts using Pine Script®, sooner or later you will run into some of the limitations we impose. This section provides you with an overview of the limitations that you may encounter.

---

## Time Limits

### Script Compilation
A **two-minute limit** is imposed on compilation time, depending on the size and complexity of your script. When compilation exceeds this limit, a warning is issued. If you receive three consecutive compilation warnings, a **one-hour ban** on compilation attempts is enforced. To avoid this, minimize script complexity by encapsulating repeated code into functions.

### Script Execution
Once compiled, the script executes across the dataset. The time allotted for the script to execute on all bars of a dataset is:
*   **20 seconds** for Basic accounts.
*   **40 seconds** for Plus, Premium, and higher tiers.

### Loop Execution
The execution time for any loop on any single bar is limited to **500 milliseconds**. The outer loop of nested loops counts as one loop, so it will time out first. 
*   *Note*: Even if loops stay under 500 ms per bar, the accumulated time across all bars can still trigger the total execution timeout. For example, a 400 ms loop running on a 20,000 bar dataset would require 8,000 seconds, which is impossible due to the 40-second script limit.

---

## Chart Visual Limits

### Plot Limits (Max 64)
A maximum of **64 plot counts** are allowed per script. The functions contributing to this count include:
*   `plot()`, `plotarrow()`, `plotbar()`, `plotcandle()`, `plotchar()`, `plotshape()`
*   `alertcondition()`, `bgcolor()`, `barcolor()`
*   `fill()` (only if its color is of the `series` form)

Drawing structures like `hline()`, `line.new()`, `label.new()`, `table.new()`, and `box.new()` **do not** generate plot counts.

One function call can generate up to five or seven plot counts depending on the parameters used:
*   `plotcandle()` with static colors uses 4 plot counts (open, high, low, close).
*   `plotcandle()` with dynamic colors (`series color`, `series wickcolor`, `series bordercolor`) can use up to **7 plot counts**.

```pinescript
//@version=6
indicator("Plot count example")

bool isUp = close > open
color isUpColor = isUp ? color.green : color.red
bool isDn = not isUp
color isDnColor = isDn ? color.red : color.green

// Uses one plot count each
p1 = plot(close, color = color.white)
p2 = plot(open, color = na)

// Uses two plot counts (series value and series color)
plot(close, color = isUpColor)

// Uses five plot counts (O, H, L, C, and series color)
plotbar(open, high, low, close, color = isUpColor)
```

### Line, Box, Polyline, and Label Limits
By default, scripts display only the last 50 drawings of each type. You can increase this cap using parameters in your script's declaration statement:
*   `max_lines_count` (Max: 500)
*   `max_boxes_count` (Max: 500)
*   `max_labels_count` (Max: 500)
*   `max_polylines_count` (Max: 100)

```pinescript
//@version=6
indicator("Label limits example", max_labels_count = 100, overlay = true)
label.new(bar_index, high, str.tostring(high, format.mintick))
```

> [!WARNING]
> Objects with properties set to `na` (e.g. `label.new(na, ...)`) **still exist** in memory and count toward your script's active drawing totals. To prevent reaching caps, use conditional structures to prevent label creation entirely:
> ```pinescript
> // ✅ Correct: Only call label.new when condition is true
> if longCondition
>     label.new(bar_index, 0, "Buy")
> ```

### Table Limits (Max 9)
Scripts can display a maximum of **nine tables** on the chart, corresponding to one for each of the nine possible screen anchoring positions (e.g. `position.top_right`). If multiple tables are assigned to the same position, only the newest one is displayed.

---

## `request.*()` Limits

### Number of Calls
A script can use up to **40 unique requests** (or up to **64 unique requests** on the Ultimate plan) to functions in the `request.*` namespace (e.g. `request.security()`, `request.security_lower_tf()`, `request.financial()`, `request.seed()`).
*   Calls to identical functions with identical arguments are consolidated and count as a single unique request.
*   Calls inside imported library scopes count toward the limit separately.
*   `request.footprint()` is limited to **one unique call per script** and is restricted to Premium and Ultimate plans.

### Intrabar Limits
Scripts can retrieve up to the most recent **100,000 lower-timeframe bars** (or up to **200,000** on the Ultimate plan) using `request.security_lower_tf()`. The retrieval length is limited using the `calc_bars_count` parameter.

### Tuple Element Limit (Max 127)
All `request.*` function calls combined in a single script cannot return more than **127 tuple elements**. 

> [!TIP]
> To request more than 127 values from a single security request, bundle the fields into a custom User-Defined Type (UDT) and pass the UDT ID instead:
> ```pinescript
> type myType
>     float v1
>     float v2
>     ...
>     float v128
> 
> // Returns a single UDT reference instead of a 128-element tuple
> myObj = request.security(syminfo.tickerid, "1D", myType.new(s1, s2, ..., s128))
> ```

---

## Script Size and Memory Limits

### Compiled Tokens (Max 100K)
The compiled intermediate language (IL) form of a script is limited to **100,000 tokens**. The total token count of all imported libraries combined cannot exceed **1 million tokens**. 
*   Unused variables, functions, and types that do not affect the output are optimized out during compilation and do not contribute to the token count.
*   To resolve token limit errors, wrap repetitive code blocks inside helper functions, or place utility functions inside libraries.

### Variables per Scope (Max 1000)
Scripts are limited to a maximum of **1,000 variables** inside any single scope (the global scope, or any local scope created inside a function, loop, or conditional block).

### Compilation Request Size (Max 5MB)
The raw code size of the script plus all imported libraries sent to the compiler cannot exceed **5MB**. Unlike token limits, this limit includes unused code because the compiler has not optimized it yet.

---

## Other Limits

### Collections
Arrays, matrices, and maps can hold a maximum of **100,000 elements**. Since maps use key-value pairs, a map is limited to **50,000 key-value pairs**.

### Historical Buffers (Max 5000)
*   The default historical buffer limit for variables and expressions using the `[]` operator is **5,000 bars** (10,000 bars for primary price series like `open`, `high`, `low`, `close`, `time`).
*   Drawings utilizing `xloc.bar_index` can reference coordinates up to **10,000 bars in the past**.

### Historical Bars Forward (Max 500)
Drawings using `xloc.bar_index` can project coordinates a maximum of **500 bars into the future** (relative to the current bar index).

```pinescript
//@version=6
indicator("Max bars forward example", overlay = true)

// Cap user-defined projections at 500 bars
int forwardBarsInput = input.int(10, "Forward Bars", minval = 1, maxval = 500)
```

### Chart Bars Count
The minimum number of historical chart bars loaded on a chart depends on the plan tier:
*   **40,000 bars**: Ultimate plan.
*   **25,000 bars**: Expert plan.
*   **20,000 bars**: Premium plan.
*   **10,000 bars**: Essential / Plus plans.
*   **5,000 bars**: Basic / other plans.

### Backtesting Trade Orders (Max 9000)
*   A strategy can place a maximum of **9,000 trade orders** during standard backtesting. Once exceeded, older trades are trimmed (garbage collected).
*   For **Deep Backtesting**, the order limit is increased to **1,000,000 orders**.
