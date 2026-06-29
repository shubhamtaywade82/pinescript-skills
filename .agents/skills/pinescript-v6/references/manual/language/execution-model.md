# Execution Model

The Pine Script® execution model dictates how a script evaluates on a chart:
- **Bar-by-Bar Execution**: The script executes once on every bar of the chart, starting from the oldest available historical bar and moving sequentially to the newest realtime bar.
- **Historical vs. Realtime**: On historical bars, the script runs only once when the bar closes. On realtime bars, the script runs on every tick.
- **Rollback**: On realtime bars, before each execution tick, all non-`varip` variables and script drawing states are rolled back to the state they were in at the close of the previous bar. Only when the realtime bar closes are the final values committed.

### Example: Tracking persistent changes

```pinescript
//@version=6
indicator("Execution Model Demo", overlay=true)

// Persistent across bars
var int barCounter = 0
barCounter := barCounter + 1

// Reset on every bar execution
int tempValue = 0
tempValue := tempValue + 1

// Persistent across realtime ticks without rollback
varip int tickCounter = 0
tickCounter := tickCounter + 1

if barstate.islast
    log.info("Bars calculated: {0}, Temp: {1}, Realtime ticks: {2}", barCounter, tempValue, tickCounter)
```

See also:
- [Type System](type-system.md)
- [Variable Declarations](variable-declarations.md)
- [Execution Model Guide](../../execution-model.md)
