# Profiling and Optimization

Pine Script® is a cloud-based compiled language geared toward efficient repeated script execution. When a user adds a Pine script to a chart, it executes numerous times, once for each available bar or tick in the data feeds it accesses, as explained in the [Execution Model](../../language/execution-model.md) page.

The Pine Script compiler automatically performs several internal optimizations to accommodate scripts of various sizes and help them run smoothly. However, such optimizations do not prevent performance bottlenecks in script executions. As such, it’s up to programmers to profile a script’s runtime performance and identify ways to modify critical code blocks and lines when they need to improve execution times.

This page covers how to profile and monitor a script’s runtime and executions with the **Pine Profiler** and explains some ways programmers can modify their code to optimize runtime performance.

---

## Pine Profiler

Before diving into optimization, it’s prudent to evaluate a script’s runtime and pinpoint bottlenecks, i.e., areas in the code that substantially impact overall performance. With these insights, programmers can ensure they focus on optimizing where it truly matters instead of spending time and effort on low-impact code.

The **Pine Profiler** is a powerful utility that analyzes the executions of all significant code lines and blocks in a script and displays performance information next to the lines inside the Pine Editor. By inspecting the Profiler’s results, programmers can gain a clearer perspective on a script’s overall runtime, the distribution of runtime across its significant code regions, and the critical portions that may need extra attention and optimization.

### Profiling a Script
The Pine Profiler can analyze the runtime performance of any editable script coded in Pine Script v6. To profile a script:
1. Add the script to the chart.
2. Open the source code in the Pine Editor.
3. Turn on the **Profiler mode** switch in the dropdown accessible via the "More" option in the top-right corner of the editor.

```pinescript
//@version=6
indicator("Pine Profiler demo")

int lengthInput = input.int(100, "Length", 2)
float upperPercentInput = input.float(75.0, "Upper percentile", 50.0, 100.0)
float lowerPercentInput = input.float(25.0, "Lower percentile", 0.0, 50.0)

float upperPercentile = ta.percentile_linear_interpolation(close, lengthInput, upperPercentInput)
float lowerPercentile = ta.percentile_linear_interpolation(close, lengthInput, lowerPercentInput)

var upperDistances = array.new<float>(lengthInput)
var lowerDistances = array.new<float>(lengthInput)

if math.abs(close - 0.5 * (upperPercentile + lowerPercentile)) > 0.5 * (upperPercentile - lowerPercentile)
    array.push(upperDistances, math.max(close - upperPercentile, 0.0))
    array.shift(upperDistances)
    array.push(lowerDistances, math.max(lowerPercentile - close, 0.0))
    array.shift(lowerDistances)

float upperAvg = upperDistances.avg()
float lowerAvg = lowerDistances.avg()
float oscillator = (upperAvg - lowerAvg) / (upperAvg + lowerAvg)

color oscColor = oscillator > 0 ?
     color.from_gradient(oscillator, 0.0, 1.0, color.gray, color.green) :
     color.from_gradient(oscillator, -1.0, 0.0, color.red, color.gray)

plot(oscillator, "Oscillator", oscColor, style = plot.style_area)
```

Once enabled, the Profiler collects execution stats and shows approximate runtime percentages next to the lines of code.

> [!NOTE]
> *   The Profiler tracks every execution of a significant code region, including executions on realtime ticks. Its information updates over time as new executions occur.
> *   Profiler results do not appear for script declaration statements, type declarations, simple variable declarations with no calculations, or unused/redundant code.
> *   When a script contains at least four significant lines of code, the Profiler highlights the top three regions with the highest performance impact using **flame icons**.

---

## Interpreting Profiled Results

### Single-Line Results
For a code line containing single-line expressions, the Profiler bar and displayed percentage represent the relative portion of the script’s total runtime spent on that line. Hovering over it displays a tooltip with:
*   **Line number**: The analyzed line.
*   **Time**: Percentage of total runtime, actual time spent, and overall script runtime.
*   **Executions**: The number of times the line evaluated.

To estimate the average time spent per execution, divide the line's time by the number of executions (e.g., `14.1 ms / 20,685 executions ≈ 0.68 microseconds`).

### Code Block Results
For lines at the start of a loop or conditional structure (`if`, `switch`), the Profiler displays stats for the **entire block**, not just the header line. Its tooltip lists:
*   **Code block range**: Range of lines included in the structure.
*   **Time**: The block's total runtime percentage.
*   **Line time**: The time spent specifically evaluating the conditional header itself (or all conditional expressions inside a `switch` / `else if` structure).
*   **Executions**: Number of times the block executed.

For `switch` or `if...else if` structures, the Profiler aggregates time for all conditions on the header line. If you need granular details for each condition, reorganize the logic into **nested `if` blocks**:

```pinescript
// Switch version (aggregated line time)
switch
    close > upperBand                     => upperBand := close
    close < lowerBand                     => lowerBand := close
    upperBand - close > close - lowerBand => upperBand := 0.9 * upperBand + 0.1 * close
    close - lowerBand > upperBand - close => lowerBand := 0.9 * lowerBand + 0.1 * close

// Equivalent nested if version (isolated line times for profiling)
if close > upperBand
    upperBand := close
else
    if close < lowerBand
        lowerBand := close
    else
        if upperBand - close > close - lowerBand
            upperBand := 0.9 * upperBand + 0.1 * close
        else
            if close - lowerBand > upperBand - close
                lowerBand := 0.9 * lowerBand + 0.1 * close
```

### User-Defined Function Calls
*   The results for each function call reflect the runtime allocated toward it and the total executions of that specific call.
*   The time and execution statistics for the code inside the function's body reflect the **combined results** of all calls made to that function.

### When Requesting Other Contexts (`request.*`)
When a script contains a user-defined function or method that executes `request.security()`, the compiler extracts the data request outside the function's scope during translation. Due to this translation, the Profiler's results for the function call line **will not** include the time spent executing the data requests.

### Insignificant, Unused, and Redundant Code
*   **Insignificant code**: Statements like simple variable declarations or `input.*()` declarations do not have a measurable performance impact and receive no stats.
*   **Unused code**: The compiler automatically removes calculations if no outputs (`plot`, `drawings`, `alerts`) depend on them. These pruned regions will not show stats.
*   **Redundant calculations**: If a script calls identical functions (like `ta.sma(close, 500)`) multiple times, the compiler evaluates it once and reuses the cached value. Only the first occurrence shows stats.
*   **Redundant functions**: If user-defined functions have identical compiled forms, the compiler retains only the first definition. Discarded clones will show no stats.

---

## Profiling Tips

### Profiling Across Configurations
For calculations where execution complexity varies based on inputs (e.g., loop range inputs), profile the script with different values. Observe how runtime scales (e.g., linearly or exponentially) to locate design inefficiencies.

### Repetitive Profiling (Dummy Input)
Since cloud execution speeds fluctuate naturally, run times will vary slightly on each reload. You can add a "dummy input" to your settings panel to easily force the script to reload and profile it multiple times without modifying the code:
```pinescript
int dummyInput = input.int(0, "Dummy input for reloading")
```

---

## Code Optimization

### 1. Using Built-ins
Pine's built-ins are highly optimized. Avoid writing custom loops for operations that built-in functions can handle.

```pinescript
//@version=6
indicator("Using built-ins demo")

// ❌ SLOW (Custom loop, ~58ms for length 20)
pineHighest(float source, int length) =>
    float result = na
    if bar_index + 1 >= length
        result := source
        if length > 1
            for i = 1 to length - 1
                result := math.max(result, source[i])
    result

// ✅ FAST (Built-in function, ~3ms for length 20)
plot(ta.highest(close, 20))
```

### 2. Reducing Repetitive Calculations
Avoid calling identical user-defined methods or functions multiple times. Store the result in a variable and reference it instead.

```pinescript
//@version=6
indicator("Reducing repetition demo")

method valuesAbove(array<float> this, int index) =>
    int result = 0
    float reference = this.get(index)
    for [i, value] in this
        if i != index and value > reference
            result += 1
    result

var array<float> data = array.new<float>(100)
data.push(close)
data.shift()

// ✅ Optimize by storing the calculation result
int count = data.valuesAbove(99)

color plotColor = switch
    count <= 10  => color.new(color.purple, 90)
    count <= 20  => color.new(color.purple, 80)
    =>              color.purple

plot(count, color = plotColor, style = plot.style_area)
```

### 3. Minimizing `request.*` Calls
Each `request.*` call adds performance overhead. Condense multiple requests from the same symbol/timeframe into a single tuple request.

```pinescript
// ❌ SLOW: 9 separate requests
float reqRank1 = request.security(sym, tf, ta.percentrank(close, 10))
float reqRank2 = request.security(sym, tf, ta.percentrank(close, 20))

// ✅ FAST: 1 request using a tuple
[r1, r2] = request.security(sym, tf, [ta.percentrank(close, 10), ta.percentrank(close, 20)])
```

### 4. Avoiding Drawing Redraws
Drawings like lines and boxes are expensive to create. Avoid deleting and recreating drawings on every bar. Use setter functions (`line.set_*()`, `box.set_*()`) to modify properties on existing objects.

### 5. Reducing Drawing Updates on History
Since historical drawings are not visible to users until the script finishes loading, restrict drawing logic to the last bar using `barstate.islast`.

```pinescript
// ✅ Only compute and update table cells on the last historical bar and realtime ticks
if barstate.islast
    for i = 0 to 19
        table.cell_set_text(infoTable, 1, i + 1, str.tostring(rsi[i]))
```

### 6. Storing Invariant Calculations (`var` / `varip`)
For calculations inside loops where inputs and values are constant across bars (e.g. weights calculations), compute them once on the first bar and save them to a persistent `var` array.

### 7. Eliminating Unnecessary Loops
Verify if a loop's calculation can be represented by a simplified math equation.
```pinescript
// ❌ Loop to calculate sum of price differences
float diffSum = 0.0
for i = 1 to length
    diffSum += source - source[i]

// ✅ Loop-free equivalent formula
float diffSumOptimized = source * length - math.sum(source, length)[1]
```

### 8. Optimizing Loops
When loops are unavoidable:
*   **Tuple Loops**: Use `for [index, item] in myCollection` instead of calling `array.indexof(myCollection, item)` inside the loop body.
*   **Loop-Invariant Code Motion**: Extract expressions that yield constant results (like `array.min(this)` or `array.range(this)`) outside of the loop's scope.

### 9. Minimizing Historical Buffer Calculations
When using dynamic lookback parameters, define suitable buffer sizes explicitly using `max_bars_back()` to prevent the script from restarting historical calculations repeatedly.
```pinescript
max_bars_back(source, 500)
```

---

## Working Around Profiler Overhead

If a script is extremely complex, the Profiler's wrapping overhead may cause it to exceed the TradingView runtime timeout (40 seconds). To profile successfully:
1. Run the script on a chart timeframe with fewer historical data points.
2. Limit the calculated range of history using the `calc_bars_count` parameter in the script declaration.

```pinescript
//@version=6
indicator("Profiler workaround", calc_bars_count = 10000)
```
