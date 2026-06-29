# Strategies

Pine Script® Strategies are specialized scripts that simulate trades across historical and realtime bars to backtest and forward test trading systems. Strategies gain access to order placement, position sizing, margin rules, and trade performance metrics via the `strategy.*` namespace.

> [!WARNING]
> Testing strategies on synthetic non-standard charts (Heikin-Ashi, Renko, Kagi, Point & Figure) yields highly unrealistic results because fills are simulated on synthetic prices. Always test on standard charts, or on Heikin-Ashi, enable the "Fill orders on standard OHLC" setting or use `fill_orders_on_standard_ohlc = true` in the `strategy()` call.

---

## Simple Strategy Example

This script places orders on moving average crossovers:
```pinescript
//@version=6
strategy("Simple strategy demo", overlay = true, margin_long = 100, margin_short = 100)

int lengthInput = input.int(14, "Base length", 2)

float fastMA = ta.sma(close, lengthInput)
float slowMA = ta.sma(close, lengthInput * 2)

if ta.crossover(fastMA, slowMA)
    strategy.entry("buy", strategy.long)

if ta.crossunder(fastMA, slowMA)
    strategy.entry("sell", strategy.short)

plot(fastMA, "Fast MA", color.aqua)
plot(slowMA, "Slow MA", color.orange)
```

---

## Broker Emulator and Bar Magnifier

### 1. Broker Emulator Assumptions
By default, the broker emulator resolves intrabar price movement using simple rules based on OHLC:
*   If `open` is closer to `high` than `low`, the path is assumed as: `open -> high -> low -> close`.
*   If `open` is closer to `low` than `high`, the path is assumed as: `open -> low -> high -> close`.
*   All prices inside the candle's range are assumed to be tradeable.

### 2. Bar Magnifier
Premium and higher plans can enable `use_bar_magnifier = true` inside the `strategy()` declaration. The emulator will use lower-timeframe data to resolve actual price movements within each chart bar, yielding precise historical backtests.

---

## Order Types

### 1. Market Orders
Places an order to execute on the next available price tick (or the open of the next bar for standard historical calculations):
```pinescript
strategy.entry("Buy", strategy.long)
```

### 2. Limit Orders
Places an order to execute only at a specified price or better:
```pinescript
strategy.entry("Long Limit", strategy.long, limit = close - 800 * syminfo.mintick)
```

### 3. Stop Orders
Triggers a market order once a target (worse) price is breached:
```pinescript
strategy.entry("Long Stop", strategy.long, stop = close + 800 * syminfo.mintick)
```

### 4. Stop-Limit Orders
Combines stop and limit arguments. Once the stop price is reached, a limit order is placed:
```pinescript
strategy.entry("Stop Limit Entry", strategy.long, stop = high, limit = low)
```

---

## Order Placement and Exits

### 1. Reversing Positions
By default, placing a `strategy.entry()` order in the opposite direction automatically sizes the order to close the current position and open the new one:
```pinescript
// If long 15 shares, this automatically sells 20 shares (closing the 15 and opening short 5)
strategy.entry("sell", strategy.short, qty = 5)
```

### 2. Pyramiding
Controls how many consecutive entries can be opened in the same direction before an exit occurs. Set `pyramiding = N` in the `strategy()` declaration statement.

### 3. Position exit Bracket orders (`strategy.exit()`)
Executes take-profit and stop-loss levels for active entries:
```pinescript
strategy.exit("exit", "buy", limit = takeProfitPrice, stop = stopLossPrice)
```

### 4. Trailing Stops
To configure trailing stops in `strategy.exit()`, specify `trail_offset` alongside either `trail_price` (absolute price) or `trail_points` (ticks):
```pinescript
strategy.exit("Trailing Stop", from_entry = "Long", trail_price = activationLevel, trail_offset = trailOffset)
```

### 5. `strategy.close()` and `strategy.close_all()`
Fires market orders to liquidate positions immediately:
```pinescript
strategy.close("buy") // Closes the entry named "buy"
strategy.close_all()  // Closes all active positions
```

---

## One-Cancels-All (OCA) Groups

Assign orders to OCA groups to manage cancellations or size reductions automatically:

### 1. `strategy.oca.cancel`
Cancels all other orders in the group once one order gets filled:
```pinescript
strategy.order("Long",  strategy.long,  stop = high, oca_name = "Entry", oca_type = strategy.oca.cancel)
strategy.order("Short", strategy.short, stop = low,  oca_name = "Entry", oca_type = strategy.oca.cancel)
```

### 2. `strategy.oca.reduce`
Reduces the quantities of other orders in the group by the size of the filled order. Used to create custom multi-level exits:
```pinescript
strategy.order("Stop",    strategy.short, stop = stopLevel,  qty = 6, oca_name = "ExitGroup", oca_type = strategy.oca.reduce)
strategy.order("Limit 1", strategy.short, limit = tpLevel1,  qty = 3, oca_name = "ExitGroup", oca_type = strategy.oca.reduce)
strategy.order("Limit 2", strategy.short, limit = tpLevel2,  qty = 6, oca_name = "ExitGroup", oca_type = strategy.oca.reduce)
```

---

## Strategy Calculation Behavior

*   **`calc_on_every_tick`**: Setting this to `true` evaluates the strategy on every tick in real-time. Because historical data doesn't contain all ticks, this setting can cause repainting.
*   **`calc_on_order_fills`**: Re-evaluates script logic immediately after any order fill, useful for dynamically shifting stops on partial exits.
*   **`process_orders_on_close`**: Processes orders at the closing tick of each bar instead of waiting for the next bar's open.

---

## Risk Management Commands

Risk checks run on every tick and cannot be bypassed dynamically:
*   `strategy.risk.allow_entry_in()`: Forces orders to execute in only one specified direction.
*   `strategy.risk.max_cons_loss_days()`: Suspends trading after a target number of consecutive losing days.
*   `strategy.risk.max_drawdown()`: Suspends trading once the specified dollar/percent drawdown limit is reached.
*   `strategy.risk.max_intraday_filled_orders()`: Restricts daily order fill frequency.
*   `strategy.risk.max_intraday_loss()`: Halts trading after daily loss limit.
*   `strategy.risk.max_position_size()`: Dynamically caps the maximum size allowed for any entry.

---

## Strategy Alert Configurations

Strategies can generate alerts using the `alert()` function or via order fill events. Use the `@strategy_alert_message` compiler annotation to specify default order fill messages:

```pinescript
//@version=6
//@strategy_alert_message {{strategy.order.action}} {{strategy.position_size}} {{ticker}} @ {{strategy.order.price}}
strategy("Alert Message Demo", overlay = true)

if ta.crossover(ta.sma(close, 5), ta.sma(close, 10))
    strategy.entry("buy", strategy.long)
```
