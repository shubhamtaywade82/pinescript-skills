# Colors

Script visuals play a critical role in the usability of indicators. Well-designed plots, fills, and drawings establish a visual hierarchy that allows the most important information to stand out, while secondary details remain unobtrusive.

Pine Script® supports 4,294,967,296 possible combinations of color and transparency (RGBA) that can be applied to lines, fills, text, candles, background space, or chart bars.

---

## The RGBA Color Model and Transparency

Each color in Pine Script is defined by four channels:
1.  **Red** (0-255)
2.  **Green** (0-255)
3.  **Blue** (0-255)
4.  **Transparency** (0-100 decimal range, representing the Alpha channel). A transparency of `0` is fully opaque, and `100` makes the color completely invisible.

---

## Constant Colors

There are 17 built-in color constants in Pine. This table lists their hexadecimal codes and RGB configurations:

| Constant Name | Hex Code | RGB Equivalents |
| :--- | :--- | :--- |
| `color.aqua` | `#00BCD4` | `color.rgb(0, 188, 212)` |
| `color.black` | `#363A45` | `color.rgb(54, 58, 69)` |
| `color.blue` | `#2196F3` | `color.rgb(33, 150, 243)` |
| `color.fuchsia` | `#E040FB` | `color.rgb(224, 64, 251)` |
| `color.gray` | `#787B86` | `color.rgb(120, 123, 134)` |
| `color.green` | `#4CAF50` | `color.rgb(76, 175, 80)` |
| `color.lime` | `#00E676` | `color.rgb(0, 230, 118)` |
| `color.maroon` | `#880E4F` | `color.rgb(136, 14, 79)` |
| `color.navy` | `#311B92` | `color.rgb(49, 27, 146)` |
| `color.olive` | `#808000` | `color.rgb(128, 128, 0)` |
| `color.orange` | `#FF9800` | `color.rgb(255, 152, 0)` |
| `color.purple` | `#9C27B0` | `color.rgb(156, 39, 176)` |
| `color.red` | `#F23645` | `color.rgb(242, 54, 69)` |
| `color.silver` | `#B2B5BE` | `color.rgb(178, 181, 190)` |
| `color.teal` | `#089981` | `color.rgb(8, 153, 129)` |
| `color.white` | `#FFFFFF` | `color.rgb(255, 255, 255)` |
| `color.yellow` | `#FDD835` | `color.rgb(253, 216, 53)` |

These three lines show equivalent ways to plot a line using the olive color with 40% transparency (60% opacity):
```pinescript
//@version=6
indicator("Constant colors demo", overlay = true)

// Hex code with Alpha suffix ('99' in hex is ~60% opacity)
plot(ta.sma(close, 10), "10-bar SMA", #80800099, 3)

// Using color.new()
plot(ta.sma(close, 30), "30-bar SMA", color.new(color.olive, 40), 3)

// Using color.rgb()
plot(ta.sma(close, 50), "50-bar SMA", color.rgb(128, 128, 0, 40), 3)
```

---

## Conditional Coloring

### 1. Simple Bull/Bear Line Coloring
Use ternary operators to alternate a plot's color dynamically:
```pinescript
//@version=6
indicator("Conditional colors", "", true)

int   lengthInput = input.int(20, "Length", minval = 2)
color maBullColorInput = input.color(color.green, "Bull")
color maBearColorInput = input.color(color.maroon, "Bear")

float ma = ta.sma(close, lengthInput)
bool maRising  = ta.rising(ma, 1)

// Alternate colors conditionally
color maColor = maRising ? maBullColorInput : maBearColorInput
plot(ma, "MA", maColor, 2)
```

### 2. Hiding Lines Using `na`
You can prevent lines from drawing between transitional bars by setting the color value to `na`:
```pinescript
//@version=6
indicator("Conditional colors", "", true)

int legsInput = input.int(5, "Pivot Legs", minval = 1)
color pHiColorInput = input.color(color.olive, "High pivots")
color pLoColorInput = input.color(color.orange, "Low pivots")

var float pHi = na
var float pLo = na

pHi := nz(ta.pivothigh(legsInput, legsInput), pHi)
pLo := nz(ta.pivotlow( legsInput, legsInput), pLo)

// Return na for the color on the bar where the pivot level shifts to avoid connecting lines
plot(pHi, "High", ta.change(pHi) != 0 ? na : pHiColorInput, 2, plot.style_line)
plot(pLo, "Low",  ta.change(pLo) != 0 ? na : pLoColorInput, 2, plot.style_line)
```

---

## Calculated Colors

You can build custom colors dynamically at runtime using three built-in functions:

### 1. `color.new()`
Used to apply transparency offsets to a base color. Below, volume columns fade to transparent depending on how many consecutive bars volume has risen or fallen:
```pinescript
//@version=6
indicator("Volume")

var color GOLD_COLOR   = #CCCC00ff
var color VIOLET_COLOR = #AA00FFff

color bullColorInput = input.color(GOLD_COLOR,   "Bull")
color bearColorInput = input.color(VIOLET_COLOR, "Bear")
int levelsInput      = input.int(10, "Gradient levels", minval = 1)

var float riseFallCnt = 0.0
riseFallCnt := math.max(1, math.min(levelsInput, riseFallCnt + math.sign(volume - nz(volume[1]))))

float transparency = 80 - math.abs(80 * riseFallCnt / levelsInput)
color volumeColor = color.new(close > open ? bullColorInput : bearColorInput, transparency)

plot(volume, "Volume", volumeColor, 1, plot.style_columns)
```

### 2. `color.rgb()`
Builds a color using raw R, G, B, and A integer values. You can extract specific channels from an existing color using `color.r()`, `color.g()`, `color.b()`, and `color.t()`:
```pinescript
//@version=6
indicator("Holiday candles", "", true)

float r = math.random(0, 255)
float g = math.random(0, 255)
float b = math.random(0, 255)
float t = math.random(0, 100)

color holidayColor = color.rgb(int(r), int(g), int(b), int(t))
plotcandle(open, high, low, close, color = holidayColor, wickcolor = holidayColor, bordercolor = holidayColor)
```

### 3. `color.from_gradient()`
Interpolates a linear color value between two boundary colors based on a numeric rank:
```pinescript
//@version=6
indicator("CCI line gradient", precision = 2)

var color GOLD_COLOR   = #CCCC00
var color BEIGE_COLOR  = #9C6E1B

float srcInput = input.source(close, title="Source")
int   lenInput = input.int(20, "Length", minval = 5)
color bullColorInput = input.color(GOLD_COLOR,  "Bull")
color bearColorInput = input.color(BEIGE_COLOR, "Bear")

float signal = ta.cci(srcInput, lenInput)

// Line color transitions between bear and bull colors based on values between -200 and 200
color signalColor = color.from_gradient(signal, -200, 200, bearColorInput, bullColorInput)
plot(signal, "CCI", signalColor)
```

---

## Donchian Channel CCI Example (Mixing Transparencies)

This script maps dynamic Donchian bands on a CCI signal. The color transparencies darken the longer a historical channel extreme has remained unbroken:

```pinescript
//@version=6
indicator("CCI DC", precision = 6)

color GOLD_COLOR   = #CCCC00ff
color VIOLET_COLOR = #AA00FFff

int lengthInput      = input.int(20, "Length", minval = 5)
color bullColorInput = input.color(GOLD_COLOR,   "Bull")
color bearColorInput = input.color(VIOLET_COLOR, "Bear")

clamp(val, min, max) =>
    math.max(min, math.min(max, val))

float v = ta.atr(int(lengthInput / 5)) / ta.atr(lengthInput * 5)
float vPct = ta.percentrank(v, lengthInput * 5)

bool highVolatility = vPct > 50
int lookBackMin = lengthInput
int lookBackMax = lengthInput * 10
var float lookBack = math.avg(lookBackMin, lookBackMax)

lookBack += highVolatility ? -2 : 2
lookBack := clamp(lookBack, lookBackMin, lookBackMax)

float signal = ta.cci(close, lengthInput)
float hiTop  = ta.highest(signal, int(lookBack))
float loBot  = ta.lowest(signal, int(lookBack))

float margin = (hiTop - loBot) / 4
float hiBot  = hiTop - margin
float loTop  = loBot + margin
float center = math.avg(hiTop, loBot)

color signalColor = color.from_gradient(signal, -200, 200, bearColorInput, bullColorInput)

float hiTransp = clamp(100 - (100 * math.max(1, nz(ta.barssince(ta.change(hiTop) != 0) + 1)) / 255), 60, 90)
float loTransp = clamp(100 - (100 * math.max(1, nz(ta.barssince(ta.change(loBot) != 0) + 1)) / 255), 60, 90)

color hiColor = color.new(bullColorInput, hiTransp)
color loColor = color.new(bearColorInput, loTransp)
color bgColor = color.new(color.gray, 100 - int(vPct / 4))

hiTopPlotID = plot(hiTop, color = na)
hiBotPlotID = plot(hiBot, color = na)
loTopPlotID = plot(loTop, color = na)
loBotPlotID = plot(loBot, color = na)

plot(signal, "CCI", signalColor, 2)
plot(center, "Centerline", color.silver, 1)

fill(hiTopPlotID, hiBotPlotID, hiColor)
fill(loTopPlotID, loBotPlotID, loColor)

bgcolor(bgColor)
```

---

## Design and Implementation Tips

### Color Picker Visibility in "Settings/Style"
Pine Script automatically populates color selector pickers inside the indicator's "Style" tab for constants or inputs that compile directly.
*   **Calculated Colors Warning**: If colors are determined at runtime using conditional series expressions (e.g. `color.new(close > open ? color.green : color.red, 50)`), the color picker **will not appear** in the Style tab.
*   **Working around this restriction**: Wrap color constants inside `color.new()` separately before making calculations:
    ```pinescript
    // ✅ Picker is visible in settings
    plotColor = close > open ? color.new(color.teal, 50) : color.new(color.red, 50)
    ```
*   `color.from_gradient()` always returns a series color, which will disable automatic Style pickers. For user-customizable setups, create dedicated `input.color()` selectors.

### Multi-Plot Outlines
To create crisp, glowing lines that stand out against dark or light background panels, plot the same series multiple times with changing opacity layers and line widths:
```pinescript
//@version=6
indicator("Neon Line Glow")
// Glow layers
plot(high, "Outer Glow", color.new(color.orange, 80), 8)
plot(high, "Inner Glow", color.new(color.orange, 60), 4)
// Core line
plot(high, "Core Line",  color.new(color.orange, 0),  1)
```
