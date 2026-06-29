# Lines and Boxes

Pine Script® facilitates drawing lines, boxes, and other geometric formations from code using the `line`, `box`, and `polyline` types. These types provide utility for programmatically drawing support and resistance levels, trend lines, price ranges, and other custom shapes.

Unlike plots, the flexibility of these types makes them suited for visualizing current calculated data at virtually any point on the chart, irrespective of the bar the script currently executes on.

Lines, boxes, and polylines are objects. Scripts reference objects of these types using IDs (pointers). Drawing IDs are qualified as `series` values, and all functions managing these objects accept `series` arguments.

> [!TIP]
> Using these drawing types often involves working with arrays, especially when creating polylines or managing active drawings. Refer to the [Arrays](../language/arrays.md) page for details.

---

## Shared Characteristics

*   **Coordinates**: Drawings can have coordinates at any location, including future coordinates beyond the last chart bar.
*   **Coord Setup**: Coordinate points are set using raw coordinates or `chart.point` instances.
*   **Time vs. Indices**: x-coordinates can represent bar indices (`xloc.bar_index`) or time values (`xloc.bar_time`).
*   **Local Scopes**: You can create and manage drawing objects within the scopes of loops and conditional structures.
*   **Garbage Collection limits**: A single script instance can display up to **500 lines**, **500 boxes**, and **100 polylines**. You can increase the maximum drawing threshold using `max_lines_count`, `max_boxes_count`, and `max_polylines_count` in the `indicator()` or `strategy()` header. When limits are exceeded, a garbage collection mechanism deletes the oldest drawings.

---

## Lines

Functions in the `line.*` namespace control the creation and management of line objects:
*   `line.new()`: Creates a new line.
*   `line.set_*()`: Modifies line properties.
*   `line.get_*()`: Retrieves values from a line instance.
*   `line.copy()`: Clones a line instance.
*   `line.delete()`: Deletes a line instance.
*   `line.all`: Read-only array containing active line IDs.

### 1. Creating Lines
The `line.new()` constructor signature supports two overloads:
```pinescript
line.new(first_point, second_point, xloc, extend, color, style, width, force_overlay) → series line
line.new(x1, y1, x2, y2, xloc, extend, color, style, width, force_overlay) → series line
```

#### Simple Line Connectors
This script draws a vertical line connecting the open and close prices at the center of each bar:
```pinescript
//@version=6
indicator("Creating lines demo", overlay = true)

firstPoint  = chart.point.now(open)
secondPoint = chart.point.now(close)

// Draw a line connecting open to close
line.new(firstPoint, secondPoint, width = 5)

bgcolor(barstate.isconfirmed ? na : color.new(color.orange, 70), title = "Unconfirmed bar highlight")
```

#### Projection Fan Example
This script draws a fan of lines projecting price ranges for the next bar:
```pinescript
//@version=6
indicator("Creating lines demo", "Simple projection fan", true, max_lines_count = 500)

int linesPerBar = input.int(20, "Line drawings per bar", 2, 100)
float step = (high - low) / (linesPerBar - 1)

firstPoint  = chart.point.from_index(bar_index - 1, hl2[1])
secondPoint = chart.point.from_index(bar_index + 1, float(na))

float barValue = low
for i = 1 to linesPerBar
    secondPoint.price := 2.0 * barValue - firstPoint.price
    color lineColor = secondPoint.price > firstPoint.price ? color.aqua : color.fuchsia
    line.new(firstPoint, secondPoint, color = lineColor)
    barValue += step

bgcolor(barstate.isconfirmed ? na : color.new(color.orange, 70), title = "Unconfirmed bar highlight")
```

### 2. Modifying Line Properties
You can update lines dynamically using setters like `line.set_first_point()`, `line.set_x1()`, or `line.set_color()`.

This example creates a line connecting a timeframe's opening price to its developing close and updates the coordinates on every bar:
```pinescript
//@version=6
indicator("Modifying lines demo", overlay = true)

string timeframe = input.timeframe("D", "Timeframe")

var line periodLine = na
var chart.point openPoint  = chart.point.now(open)
var chart.point closePoint = chart.point.now(close)

if timeframe.change(timeframe)
    color finalColor = closePoint.price > openPoint.price ? color.green : color.red
    line.set_color(periodLine, finalColor)

    openPoint  := chart.point.now(open)
    closePoint := chart.point.now(close)
    periodLine := line.new(openPoint, closePoint, xloc.bar_time, style = line.style_arrow_right, width = 3)
else if not na(periodLine)
    closePoint := chart.point.now(close)
    color developingColor = closePoint.price > openPoint.price ? color.aqua : color.fuchsia
    
    periodLine.set_second_point(closePoint)
    periodLine.set_color(developingColor)
```

### 3. Line Styles
The `style` parameter accepts the following style flags:
*   `line.style_solid`
*   `line.style_dotted`
*   `line.style_dashed`
*   `line.style_arrow_left`
*   `line.style_arrow_right`
*   `line.style_arrow_both`

### 4. Reading Line Values
Getters like `line.get_x1()`, `line.get_y1()`, or `line.get_price()` extract information from line objects.
`line.get_price()` calculates the exact y-value of a line at any bar index:
```pinescript
float lineValue = line.get_price(directionLine, bar_index)
```

---

## Boxes

Functions in the `box.*` namespace manage rectangle drawings:
*   `box.new()`: Creates a new box.
*   `box.set_*()`: Modifies box properties.
*   `box.get_*()`: Retrieves coordinates or text parameters.
*   `box.copy()`: Clones a box instance.
*   `box.delete()`: Deletes a box instance.
*   `box.all`: Read-only array referencing active boxes.

### 1. Creating Boxes
The `box.new()` constructor signature supports two overloads:
```pinescript
box.new(top_left, bottom_right, border_color, border_width, border_style, extend, xloc, bgcolor, text, text_size, text_color, text_halign, text_valign, text_wrap, text_font_family, force_overlay, text_formatting) → series box
box.new(left, top, right, bottom, border_color, border_width, border_style, extend, xloc, bgcolor, text, text_size, text_color, text_halign, text_valign, text_wrap, text_font_family, force_overlay, text_formatting) → series box
```

#### Simple Box Overlays
This script draws a box bounding the high and low range of each bar:
```pinescript
//@version=6
indicator("Creating boxes demo", overlay = true)

topLeft     = chart.point.now(high)
bottomRight = chart.point.from_index(bar_index + 1, low)

box.new(topLeft, bottomRight, color.purple, 2, bgcolor = color.new(color.gray, 70))
```

### 2. Modifying Box Properties
This script tracks the highest volume bars on a timeframe and projects their range:
```pinescript
//@version=6
indicator("Modifying boxes demo", "High volume boxes", true, max_boxes_count = 100)

string timeframe = input.timeframe("D", "Timeframe")

var box upBox = na
var box downBox = na
var float upVolume = na
var float downVolume = na

int closeTime = time_close(timeframe)
bool changeTF = timeframe.change(timeframe)

topLeft     = chart.point.now(high)
bottomRight = chart.point.from_time(closeTime, low)

if changeTF and not na(volume)
    if close > open
        upVolume   := volume
        downVolume := 0.0
        upBox   := box.new(topLeft, bottomRight, color.teal, 3, xloc = xloc.bar_time, bgcolor = color.new(color.teal, 90))
        downBox := box.new(na, na, na, na, color.maroon, 3, xloc = xloc.bar_time, bgcolor = color.new(color.maroon, 90))
    else
        upVolume   := 0.0
        downVolume := volume
        upBox   := box.new(na, na, na, na, color.teal, 3, xloc = xloc.bar_time, bgcolor = color.new(color.teal, 90))
        downBox := box.new(topLeft, bottomRight, color.maroon, 3, xloc = xloc.bar_time, bgcolor = color.new(color.maroon, 90))
else if close > open and volume > upVolume
    upVolume := volume
    box.set_top_left_point(upBox, topLeft)
    box.set_bottom_right_point(upBox, bottomRight)
else if close <= open and volume > downVolume
    downVolume := volume
    box.set_top_left_point(downBox, topLeft)
    box.set_bottom_right_point(downBox, bottomRight)
```

---

## Polylines

Polylines sequentially connect an array of coordinates using straight or curved segments. A single polyline can contain up to 10,000 points.

### 1. Creating Polylines
The constructor signature is:
```pinescript
polyline.new(points, curved, closed, xloc, line_color, fill_color, line_style, line_width, force_overlay) → series polyline
```

#### Basic Polyline Connector
```pinescript
//@version=6
indicator("Creating polylines demo", "Oscillating polyline")

int length = input.int(20, "Length between points", 2)
var points = array.new<chart.point>()
var int yValue = 1

bool newPoint = bar_index % length == 0

if newPoint
    points.push(chart.point.now(yValue))
    yValue *= -1

if barstate.islastconfirmedhistory
    polyline.new(points, xloc = xloc.bar_time, line_color = #9151A6, line_width = 3)
```

### 2. Curved Polylines
Setting `curved = true` interpolates a piecewise spline function between coordinate nodes to draw smooth waves:
```pinescript
polyline.new(points, curved = true, xloc = xloc.bar_time, line_color = #9151A6, line_width = 3)
```

### 3. Closed Polygonal Shapes
Setting `closed = true` links the final array index back to the starting index:
```pinescript
//@version=6
indicator("Closed shapes demo", "N-sided polygons", true)

float xScale = input.float(3.0, "X scale", 1.0)
float yScale = input.float(1.0, "Y scale") * ta.atr(2)

var points = array.new<chart.point>()
bool newPolygon = bar_index % int(math.round(2 * xScale)) == 0 and barstate.isconfirmed

if newPolygon
    points.clear()
    int numberOfSides = int(math.random(3, 7))
    float rotationOffset = math.random(0.0, 2.0) * math.pi
    float step = 2 * math.pi / numberOfSides
    float angle = rotationOffset

    for i = 1 to numberOfSides
        int xValue = int(math.round(xScale * math.cos(angle))) + bar_index
        float yValue = yScale * math.sin(angle) + hl2
        points.push(chart.point.from_index(xValue, yValue))
        angle += step

    polyline.new(points, closed = true, line_color = color.navy, fill_color = color.new(color.orange, 50), line_width = 3)
```

---

## Realtime Executions & Rollbacks

When a script executes on an unconfirmed (realtime) bar, drawing creation calls (`line.new()`, `box.new()`) execute on every tick update. To prevent the chart from cluttering, Pine Script automatically **rolls back** (deletes) any drawings generated during the unconfirmed bar before evaluating the next tick. The drawing is only committed once the bar closes.

---

## Technical Restrictions & Limitations

*   **Past/Future limits**: Coordinates using `xloc.bar_index` must stay within **10,000 bars in the past** and **500 bars in the future**.
*   **Historical Buffer Errors**: If you access coordinates deep in history on realtime bars, you must explicitly set the historical buffer size:
    ```pinescript
    // Explicitly set the historical buffer of the time series to 300 bars
    max_bars_back(time, 300)
    ```
*   **Context constraints**: Drawing objects cannot be created inside `request.*()` calls or when using the `timeframe` indicator argument.
