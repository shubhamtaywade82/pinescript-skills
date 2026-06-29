# Pine Script® v6 Variables Reference

Source: https://www.tradingview.com/pine-script-reference/v6/

## [to track the take-profit and stop-loss price.](https://www.tradingview.com/pine-script-reference/v6/#var_totrackthetake-profitandstop-lossprice.)

to track the take-profit and stop-loss price.
var float takeProfit = na
var float stopLoss   = na

## [// Calculate a 14-bar and 28-bar moving average of `close` prices.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatea14-barand28-barmovingaverageofcloseprices.)

// Calculate a 14-bar and 28-bar moving average of `close` prices.
float sma14 = ta.sma(close, 14)
float sma28 = ta.sma(close, 28)

## [if ta.crossover(sma14, sma28) and strategy.opentrades == 0](https://www.tradingview.com/pine-script-reference/v6/#var_ifta.crossoversma14sma28andstrategy.opentrades0)

if ta.crossover(sma14, sma28) and strategy.opentrades == 0
    // Place a market order to enter a long position.
    strategy.entry("My Long Entry ID", strategy.long)
    // Place a take-profit and stop-loss order when the entry order fills.
    strategy.exit("My Long Exit ID", "My Long Entry ID", profit = profitDistanceInput, loss = lossDistanceInput)

## [if ta.change(strategy.opentrades) == 1](https://www.tradingview.com/pine-script-reference/v6/#var_ifta.changestrategy.opentrades1)

if ta.change(strategy.opentrades) == 1
    //@variable The long entry price.
    float entryPrice = strategy.opentrades.entry_price(0)
    // Update the `takeProfit` and `stopLoss` values.
    takeProfit := entryPrice + profitDistanceInput *syminfo.mintick
    stopLoss   := entryPrice - lossDistanceInput* syminfo.mintick

## [if ta.change(strategy.closedtrades) == 1](https://www.tradingview.com/pine-script-reference/v6/#var_ifta.changestrategy.closedtrades1)

if ta.change(strategy.closedtrades) == 1
    // Reset the `takeProfit` and `stopLoss`.
    takeProfit := na
    stopLoss   := na

## [// Plot the `takeProfit` and `stopLoss`.](https://www.tradingview.com/pine-script-reference/v6/#var_PlotthetakeProfitandstopLoss.)

// Plot the `takeProfit` and `stopLoss`.
plot(takeProfit, "Take-profit level", color.green, 2, plot.style_linebr)
plot(stopLoss, "Stop-loss level", color.red, 2, plot.style_linebr)
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
strategy("Trailing stop strategy", overlay = true)

## [//@variable The distance required to activate the trailing stop.](https://www.tradingview.com/pine-script-reference/v6/#var_variableThedistancerequiredtoactivatethetrailingstop.)

//@variable The distance required to activate the trailing stop.
float activationDistanceInput = input.int(100, "Trail activation distance, in ticks") * syminfo.mintick
//@variable The number of ticks the trailing stop follows behind the price as it reaches new peaks.
int trailDistanceInput = input.int(100, "Trail distance, in ticks")

## [//@function Draws a label and line at the specified `price` to visualize a trailing stop order's activation level.](https://www.tradingview.com/pine-script-reference/v6/#var_functionDrawsalabelandlineatthespecifiedpricetovisualizeatrailingstopordersactivationlevel.)

//@function Draws a label and line at the specified `price` to visualize a trailing stop order's activation level.
drawActivation(float price) =>
    label.new(
         bar_index, price, "Activation level", style = label.style_label_right,
         color = color.gray, textcolor = color.white
     )
    line.new(
         bar_index, price, bar_index + 1, price, extend = extend.right, style = line.style_dashed, color = color.gray
     )

## [//@function Stops the `l` line from extending further.](https://www.tradingview.com/pine-script-reference/v6/#var_functionStopsthellinefromextendingfurther.)

//@function Stops the `l` line from extending further.
method stopExtend(line l) =>
    l.set_x2(bar_index)
    l.set_extend(extend.none)

## [// The activation line, active trailing stop price, and active trailing stop flag.](https://www.tradingview.com/pine-script-reference/v6/#var_Theactivationlineactivetrailingstoppriceandactivetrailingstopflag.)

// The activation line, active trailing stop price, and active trailing stop flag.
var line activationLine     = na
var float trailingStopPrice = na
var bool isActive           = false

## [if bar_index % 100 == 0 and strategy.opentrades == 0](https://www.tradingview.com/pine-script-reference/v6/#var_ifbar_index1000andstrategy.opentrades0)

if bar_index % 100 == 0 and strategy.opentrades == 0
    trailingStopPrice := na
    isActive          := false
    // Place a market order to enter a long position.
    strategy.entry("My Long Entry ID", strategy.long)
    //@variable The activation level's price.
    float activationPrice = close + activationDistanceInput
    // Create a trailing stop order that activates the defined number of ticks above the entry price.
    strategy.exit(
         "My Long Exit ID", "My Long Entry ID", trail_price = activationPrice, trail_offset = trailDistanceInput,
         oca_name = "Exit"
     )
    // Create new drawings at the `activationPrice`.
    activationLine := drawActivation(activationPrice)

## [// Logic for trailing stop visualization.](https://www.tradingview.com/pine-script-reference/v6/#var_Logicfortrailingstopvisualization.)

// Logic for trailing stop visualization.
if strategy.opentrades == 1
    // Stop extending the `activationLine` when the stop activates.
    if not isActive and high > activationLine.get_price(bar_index)
        isActive := true
        activationLine.stopExtend()
    // Update the `trailingStopPrice` while the trailing stop is active.
    if isActive
        float offsetPrice = high - trailDistanceInput * syminfo.mintick
        trailingStopPrice := math.max(nz(trailingStopPrice, offsetPrice), offsetPrice)

## [// Close the trade with a market order if the trailing stop does not activate before the next 300th bar.](https://www.tradingview.com/pine-script-reference/v6/#var_Closethetradewithamarketorderifthetrailingstopdoesnotactivatebeforethenext300thbar.)

// Close the trade with a market order if the trailing stop does not activate before the next 300th bar.
if not isActive and bar_index % 300 == 0
    strategy.close_all("Market close")

## [// Reset the `trailingStopPrice` and `isActive` flags when the trade closes, and stop extending the `activationLine`.](https://www.tradingview.com/pine-script-reference/v6/#var_ResetthetrailingStopPriceandisActiveflagswhenthetradeclosesandstopextendingtheactivationLine.)

// Reset the `trailingStopPrice` and `isActive` flags when the trade closes, and stop extending the `activationLine`.
if ta.change(strategy.closedtrades) > 0
    if not isActive
        activationLine.stopExtend()
    trailingStopPrice := na
    isActive          := false

## [// Plot the `trailingStopPrice`.](https://www.tradingview.com/pine-script-reference/v6/#var_PlotthetrailingStopPrice.)

// Plot the `trailingStopPrice`.
plot(trailingStopPrice, "Trailing stop", color.red, 3, plot.style_linebr)
Remarks
A single call to the strategy.exit() command can generate exit orders for several entries in an open position, depending on the call's from_entry value. If the call does not include a from_entry argument, it creates exit orders for all open trades, even the ones opened after the call, until the position closes. See this section of our User Manual to learn more.
When a position consists of several open trades, and the close_entries_rule in the strategy() declaration statement is "FIFO" (default), the orders from a strategy.exit() call exit from the position starting with the first open trade. This behavior applies even if the from_entry value is the entry ID of different open trades. However, in that case, the maximum size of the exit orders still depends on the trades opened by orders with the from_entry ID. For more information, see this section of our User Manual.
If a strategy.exit() call includes arguments for creating stop-loss and trailing stop orders, the command places only the order that is supposed to fill first, because both orders are of the "stop" type.
strategy.opentrades.commission()

## [Returns the sum of entry and exit fees paid in the open trade, expressed in strategy.account_currency.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnsthesumofentryandexitfeespaidintheopentradeexpressedinstrategy.account_currency.)

Returns the sum of entry and exit fees paid in the open trade, expressed in strategy.account_currency.
Syntax
strategy.opentrades.commission(trade_num) → series float
Arguments
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
Example

## [// Calculates the gross profit or loss for the current open position.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatesthegrossprofitorlossforthecurrentopenposition.)

// Calculates the gross profit or loss for the current open position.
//@version=6
strategy("`strategy.opentrades.commission` Example", commission_type = strategy.commission.percent, commission_value = 0.1)

## [// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.](https://www.tradingview.com/pine-script-reference/v6/#var_Strategycallstoenterlongtradesevery15barsandexitlongtradesevery20bars.)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

## [// Calculate gross profit or loss for open positions only.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculategrossprofitorlossforopenpositionsonly.)

// Calculate gross profit or loss for open positions only.
tradeOpenGrossPL() =>
    sumOpenGrossPL = 0.0
    for tradeNo = 0 to strategy.opentrades - 1
        sumOpenGrossPL += strategy.opentrades.profit(tradeNo) - strategy.opentrades.commission(tradeNo)
    result = sumOpenGrossPL

## [plot(tradeOpenGrossPL())](https://www.tradingview.com/pine-script-reference/v6/#var_plottradeOpenGrossPL)

plot(tradeOpenGrossPL())
See also
strategy()
strategy.closedtrades.commission()
strategy.opentrades.entry_bar_index()

## [Returns the bar_index of the open trade's entry.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnsthebar_indexoftheopentradesentry.)

Returns the bar_index of the open trade's entry.
Syntax
strategy.opentrades.entry_bar_index(trade_num) → series int
Arguments
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
Example

## [// Wait 10 bars and then close the position.](https://www.tradingview.com/pine-script-reference/v6/#var_Wait10barsandthenclosetheposition.)

// Wait 10 bars and then close the position.
//@version=6
strategy("`strategy.opentrades.entry_bar_index` Example")

## [barsSinceLastEntry() =>](https://www.tradingview.com/pine-script-reference/v6/#var_barsSinceLastEntry)

barsSinceLastEntry() =>
    strategy.opentrades > 0 ? bar_index - strategy.opentrades.entry_bar_index(strategy.opentrades - 1) : na

## [// Enter a long position if there are no open positions.](https://www.tradingview.com/pine-script-reference/v6/#var_Enteralongpositioniftherearenoopenpositions.)

// Enter a long position if there are no open positions.
if strategy.opentrades == 0
    strategy.entry("Long", strategy.long)

## [// Close the long position after 10 bars.](https://www.tradingview.com/pine-script-reference/v6/#var_Closethelongpositionafter10bars.)

// Close the long position after 10 bars.
if barsSinceLastEntry() >= 10
    strategy.close("Long")
See also
strategy.closedtrades.entry_bar_index()
strategy.closedtrades.exit_bar_index()
strategy.opentrades.entry_comment()

## [Returns the comment message of the open trade's entry, or na if there is no entry with this trade_num.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnsthecommentmessageoftheopentradesentryornaifthereisnoentrywiththistrade_num.)

Returns the comment message of the open trade's entry, or na if there is no entry with this trade_num.
Syntax
strategy.opentrades.entry_comment(trade_num) → series string
Arguments
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
strategy("`strategy.opentrades.entry_comment()` Example", overlay = true)

## [stopPrice = open * 1.01](https://www.tradingview.com/pine-script-reference/v6/#var_stopPriceopen1.01)

stopPrice = open * 1.01

## [longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))](https://www.tradingview.com/pine-script-reference/v6/#var_longConditionta.crossoverta.smaclose14ta.smaclose28)

longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))

## [if (longCondition)](https://www.tradingview.com/pine-script-reference/v6/#var_iflongCondition)

if (longCondition)
    strategy.entry("Long", strategy.long, stop = stopPrice, comment = str.tostring(stopPrice, "#.####"))

## [var testTable = table.new(position.top_right, 1, 3, color.orange, border_width = 1)](https://www.tradingview.com/pine-script-reference/v6/#var_vartestTabletable.newposition.top_right13color.orangeborder_width1)

var testTable = table.new(position.top_right, 1, 3, color.orange, border_width = 1)

## [if barstate.islastconfirmedhistory or barstate.isrealtime](https://www.tradingview.com/pine-script-reference/v6/#var_ifbarstate.islastconfirmedhistoryorbarstate.isrealtime)

if barstate.islastconfirmedhistory or barstate.isrealtime
    table.cell(testTable, 0, 0, 'Last entry stats')
    table.cell(testTable, 0, 1, "Order stop price value: " + strategy.opentrades.entry_comment(strategy.opentrades - 1))
    table.cell(testTable, 0, 2, "Actual Entry Price: " + str.tostring(strategy.opentrades.entry_price(strategy.opentrades - 1)))
See also
strategy()
strategy.entry()
strategy.opentrades
strategy.opentrades.entry_id()

## [Returns the id of the open trade's entry.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnstheidoftheopentradesentry.)

Returns the id of the open trade's entry.
Syntax
strategy.opentrades.entry_id(trade_num) → series string
Arguments
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
strategy("`strategy.opentrades.entry_id` Example", overlay = true)

## [// We enter a long position when 14 period sma crosses over 28 period sma.](https://www.tradingview.com/pine-script-reference/v6/#var_Weenteralongpositionwhen14periodsmacrossesover28periodsma.)

// We enter a long position when 14 period sma crosses over 28 period sma.
// We enter a short position when 14 period sma crosses under 28 period sma.
longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))
shortCondition = ta.crossunder(ta.sma(close, 14), ta.sma(close, 28))

## [// Strategy calls to enter a long or short position when the corresponding condition is met.](https://www.tradingview.com/pine-script-reference/v6/#var_Strategycallstoenteralongorshortpositionwhenthecorrespondingconditionismet.)

// Strategy calls to enter a long or short position when the corresponding condition is met.
if longCondition
    strategy.entry("Long entry at bar #" + str.tostring(bar_index), strategy.long)
if shortCondition
    strategy.entry("Short entry at bar #" + str.tostring(bar_index), strategy.short)

## [// Display ID of the latest open position.](https://www.tradingview.com/pine-script-reference/v6/#var_DisplayIDofthelatestopenposition.)

// Display ID of the latest open position.
if barstate.islastconfirmedhistory
    label.new(bar_index, high + (2 * ta.tr), "Last opened position is \n " + strategy.opentrades.entry_id(strategy.opentrades - 1))
Returns
Returns the id of the open trade's entry.
Remarks
The function returns na if trade_num is not in the range: 0 to strategy.opentrades-1.
See also
strategy.opentrades.entry_bar_index()
strategy.opentrades.entry_price()
strategy.opentrades.entry_time()
strategy.opentrades.entry_price()

## [Returns the price of the open trade's entry.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnsthepriceoftheopentradesentry.)

Returns the price of the open trade's entry.
Syntax
strategy.opentrades.entry_price(trade_num) → series float
Arguments
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
strategy("strategy.opentrades.entry_price Example 1", overlay = true)

## [// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.](https://www.tradingview.com/pine-script-reference/v6/#var_Strategycallstoenterlongtradesevery15barsandexitlongtradesevery20bars.)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if ta.crossover(close, ta.sma(close, 14))
    strategy.entry("Long", strategy.long)

## [// Return the entry price for the latest closed trade.](https://www.tradingview.com/pine-script-reference/v6/#var_Returntheentrypriceforthelatestclosedtrade.)

// Return the entry price for the latest closed trade.
currEntryPrice = strategy.opentrades.entry_price(strategy.opentrades - 1)
currExitPrice = currEntryPrice * 1.05

## [if high >= currExitPrice](https://www.tradingview.com/pine-script-reference/v6/#var_ifhighcurrExitPrice)

if high >= currExitPrice
    strategy.close("Long")

## [plot(currEntryPrice, "Long entry price", style = plot.style_linebr)](https://www.tradingview.com/pine-script-reference/v6/#var_plotcurrEntryPriceLongentrypricestyleplot.style_linebr)

plot(currEntryPrice, "Long entry price", style = plot.style_linebr)
plot(currExitPrice, "Long exit price", color.green, style = plot.style_linebr)
Example

## [// Calculates the average price for the open position.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatestheaveragepricefortheopenposition.)

// Calculates the average price for the open position.
//@version=6
strategy("strategy.opentrades.entry_price Example 2", pyramiding = 2)

## [// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.](https://www.tradingview.com/pine-script-reference/v6/#var_Strategycallstoenterlongtradesevery15barsandexitlongtradesevery20bars.)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

## [// Calculates the average price for the open position.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatestheaveragepricefortheopenposition.)

// Calculates the average price for the open position.
avgOpenPositionPrice() =>
    sumOpenPositionPrice = 0.0
    for tradeNo = 0 to strategy.opentrades - 1
        sumOpenPositionPrice += strategy.opentrades.entry_price(tradeNo) * strategy.opentrades.size(tradeNo) / strategy.position_size
    result = nz(sumOpenPositionPrice / strategy.opentrades)

## [plot(avgOpenPositionPrice())](https://www.tradingview.com/pine-script-reference/v6/#var_plotavgOpenPositionPrice)

plot(avgOpenPositionPrice())
See also
strategy.closedtrades.exit_price()
strategy.opentrades.entry_time()

## [Returns the UNIX time of the open trade's entry, expressed in milliseconds.](https://www.tradingview.com/pine-script-reference/v6/#var_ReturnstheUNIXtimeoftheopentradesentryexpressedinmilliseconds.)

Returns the UNIX time of the open trade's entry, expressed in milliseconds.
Syntax
strategy.opentrades.entry_time(trade_num) → series int
Arguments
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
strategy("strategy.opentrades.entry_time Example")

## [// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.](https://www.tradingview.com/pine-script-reference/v6/#var_Strategycallstoenterlongtradesevery15barsandexitlongtradesevery20bars.)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

## [// Calculates duration in milliseconds since the last position was opened.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatesdurationinmillisecondssincethelastpositionwasopened.)

// Calculates duration in milliseconds since the last position was opened.
timeSinceLastEntry()=>
    strategy.opentrades > 0 ? (time - strategy.opentrades.entry_time(strategy.opentrades - 1)) : na

## [plot(timeSinceLastEntry() / 1000 *60* 60 * 24, "Days since last entry")](https://www.tradingview.com/pine-script-reference/v6/#var_plottimeSinceLastEntry1000606024Dayssincelastentry)

plot(timeSinceLastEntry() / 1000 *60* 60 * 24, "Days since last entry")
See also
strategy.closedtrades.entry_time()
strategy.closedtrades.exit_time()
strategy.opentrades.max_drawdown()

## [Returns the maximum drawdown of the open trade, i.e., the maximum possible loss during the trade, expressed in strategy.account_currency.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnsthemaximumdrawdownoftheopentradei.e.themaximumpossiblelossduringthetradeexpressedinstrategy.account_currency.)

Returns the maximum drawdown of the open trade, i.e., the maximum possible loss during the trade, expressed in strategy.account_currency.
Syntax
strategy.opentrades.max_drawdown(trade_num) → series float
Arguments
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
strategy("strategy.opentrades.max_drawdown Example 1")

## [// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.](https://www.tradingview.com/pine-script-reference/v6/#var_Strategycallstoenterlongtradesevery15barsandexitlongtradesevery20bars.)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

## [// Plot the max drawdown of the latest open trade.](https://www.tradingview.com/pine-script-reference/v6/#var_Plotthemaxdrawdownofthelatestopentrade.)

// Plot the max drawdown of the latest open trade.
plot(strategy.opentrades.max_drawdown(strategy.opentrades - 1), "Max drawdown of the latest open trade")
Example

## [// Calculates the max trade drawdown value for all open trades.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatesthemaxtradedrawdownvalueforallopentrades.)

// Calculates the max trade drawdown value for all open trades.
//@version=6
strategy("`strategy.opentrades.max_drawdown` Example 2", pyramiding = 100)

## [// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.](https://www.tradingview.com/pine-script-reference/v6/#var_Strategycallstoenterlongtradesevery15barsandexitlongtradesevery20bars.)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

## [// Get the biggest max trade drawdown value from all of the open trades.](https://www.tradingview.com/pine-script-reference/v6/#var_Getthebiggestmaxtradedrawdownvaluefromalloftheopentrades.)

// Get the biggest max trade drawdown value from all of the open trades.
maxTradeDrawDown() =>
    maxDrawdown = 0.0
    for tradeNo = 0 to strategy.opentrades - 1
        maxDrawdown := math.max(maxDrawdown, strategy.opentrades.max_drawdown(tradeNo))
    result = maxDrawdown

## [plot(maxTradeDrawDown(), "Biggest max drawdown")](https://www.tradingview.com/pine-script-reference/v6/#var_plotmaxTradeDrawDownBiggestmaxdrawdown)

plot(maxTradeDrawDown(), "Biggest max drawdown")
Remarks
The function returns na if trade_num is not in the range: 0 to strategy.closedtrades - 1.
See also
strategy.closedtrades.max_drawdown()
strategy.max_drawdown
strategy.opentrades.max_drawdown_percent()

## [Returns the maximum drawdown of the open trade, i.e., the maximum possible loss during the trade, expressed as a percentage and calculated by formula: Lowest Value During Trade / (Entry Price x Quantity) * 100.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnsthemaximumdrawdownoftheopentradei.e.themaximumpossiblelossduringthetradeexpressedasapercentageandcalculatedbyformulaLowestValueDuringTradeEntryPricexQuantity100.)

Returns the maximum drawdown of the open trade, i.e., the maximum possible loss during the trade, expressed as a percentage and calculated by formula: Lowest Value During Trade / (Entry Price x Quantity) * 100.
Syntax
strategy.opentrades.max_drawdown_percent(trade_num) → series float
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
See also
strategy.opentrades.max_drawdown()
strategy.max_drawdown
strategy.opentrades.max_runup()

## [Returns the maximum run up of the open trade, i.e., the maximum possible profit during the trade, expressed in strategy.account_currency.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnsthemaximumrunupoftheopentradei.e.themaximumpossibleprofitduringthetradeexpressedinstrategy.account_currency.)

Returns the maximum run up of the open trade, i.e., the maximum possible profit during the trade, expressed in strategy.account_currency.
Syntax
strategy.opentrades.max_runup(trade_num) → series float
Arguments
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
strategy("strategy.opentrades.max_runup Example 1")

## [// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.](https://www.tradingview.com/pine-script-reference/v6/#var_Strategycallstoenterlongtradesevery15barsandexitlongtradesevery20bars.)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

## [// Plot the max runup of the latest open trade.](https://www.tradingview.com/pine-script-reference/v6/#var_Plotthemaxrunupofthelatestopentrade.)

// Plot the max runup of the latest open trade.
plot(strategy.opentrades.max_runup(strategy.opentrades - 1), "Max runup of the latest open trade")
Example

## [// Calculates the max trade runup value for all open trades.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatesthemaxtraderunupvalueforallopentrades.)

// Calculates the max trade runup value for all open trades.
//@version=6
strategy("strategy.opentrades.max_runup Example 2", pyramiding = 100)

## [// Enter a long position every 30 bars.](https://www.tradingview.com/pine-script-reference/v6/#var_Enteralongpositionevery30bars.)

// Enter a long position every 30 bars.
if bar_index % 30 == 0
    strategy.entry("Long", strategy.long)

## [// Calculate biggest max trade runup value from all of the open trades.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatebiggestmaxtraderunupvaluefromalloftheopentrades.)

// Calculate biggest max trade runup value from all of the open trades.
maxOpenTradeRunUp() =>
    maxRunup = 0.0
    for tradeNo = 0 to strategy.opentrades - 1
        maxRunup := math.max(maxRunup, strategy.opentrades.max_runup(tradeNo))
    result = maxRunup

## [plot(maxOpenTradeRunUp(), "Biggest max runup of all open trades")](https://www.tradingview.com/pine-script-reference/v6/#var_plotmaxOpenTradeRunUpBiggestmaxrunupofallopentrades)

plot(maxOpenTradeRunUp(), "Biggest max runup of all open trades")
See also
strategy.closedtrades.max_runup()
strategy.max_drawdown
strategy.opentrades.max_runup_percent()

## [Returns the maximum run-up of the open trade, i.e., the maximum possible profit during the trade, expressed as a percentage and calculated by formula: Highest Value During Trade / (Entry Price x Quantity) * 100.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnsthemaximumrun-upoftheopentradei.e.themaximumpossibleprofitduringthetradeexpressedasapercentageandcalculatedbyformulaHighestValueDuringTradeEntryPricexQuantity100.)

Returns the maximum run-up of the open trade, i.e., the maximum possible profit during the trade, expressed as a percentage and calculated by formula: Highest Value During Trade / (Entry Price x Quantity) * 100.
Syntax
strategy.opentrades.max_runup_percent(trade_num) → series float
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
See also
strategy.opentrades.max_runup()
strategy.max_runup
strategy.opentrades.profit()

## [Returns the profit/loss of the open trade, expressed in strategy.account_currency. Losses are expressed as negative values.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnstheprofitlossoftheopentradeexpressedinstrategy.account_currency.Lossesareexpressedasnegativevalues.)

Returns the profit/loss of the open trade, expressed in strategy.account_currency. Losses are expressed as negative values.
Syntax
strategy.opentrades.profit(trade_num) → series float
Arguments
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
Example

## [// Returns the profit of the last open trade.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnstheprofitofthelastopentrade.)

// Returns the profit of the last open trade.
//@version=6
strategy("`strategy.opentrades.profit` Example 1", commission_type = strategy.commission.percent, commission_value = 0.1)

## [// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.](https://www.tradingview.com/pine-script-reference/v6/#var_Strategycallstoenterlongtradesevery15barsandexitlongtradesevery20bars.)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

## [plot(strategy.opentrades.profit(strategy.opentrades - 1), "Profit of the latest open trade")](https://www.tradingview.com/pine-script-reference/v6/#var_plotstrategy.opentrades.profitstrategy.opentrades-1Profitofthelatestopentrade)

plot(strategy.opentrades.profit(strategy.opentrades - 1), "Profit of the latest open trade")
Example

## [// Calculates the profit for all open trades.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatestheprofitforallopentrades.)

// Calculates the profit for all open trades.
//@version=6
strategy("`strategy.opentrades.profit` Example 2", pyramiding = 5)

## [// Strategy calls to enter 5 long positions every 2 bars.](https://www.tradingview.com/pine-script-reference/v6/#var_Strategycallstoenter5longpositionsevery2bars.)

// Strategy calls to enter 5 long positions every 2 bars.
if bar_index % 2 == 0
    strategy.entry("Long", strategy.long, qty = 5)

## [// Calculate open profit or loss for the open positions.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculateopenprofitorlossfortheopenpositions.)

// Calculate open profit or loss for the open positions.
tradeOpenPL() =>
    sumProfit = 0.0
    for tradeNo = 0 to strategy.opentrades - 1
        sumProfit += strategy.opentrades.profit(tradeNo)
    result = sumProfit

## [plot(tradeOpenPL(), "Profit of all open trades")](https://www.tradingview.com/pine-script-reference/v6/#var_plottradeOpenPLProfitofallopentrades)

plot(tradeOpenPL(), "Profit of all open trades")
See also
strategy.closedtrades.profit()
strategy.openprofit
strategy.netprofit
strategy.grossprofit
strategy.opentrades.profit_percent()

## [Returns the profit/loss of the open trade, expressed as a percentage. Losses are expressed as negative values.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnstheprofitlossoftheopentradeexpressedasapercentage.Lossesareexpressedasnegativevalues.)

Returns the profit/loss of the open trade, expressed as a percentage. Losses are expressed as negative values.
Syntax
strategy.opentrades.profit_percent(trade_num) → series float
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
See also
strategy.opentrades.profit()
strategy.opentrades.size()

## [Returns the direction and the number of contracts traded in the open trade. If the value is > 0, the market position was long. If the value is < 0, the market position was short.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnsthedirectionandthenumberofcontractstradedintheopentrade.Ifthevalueis0themarketpositionwaslong.Ifthevalueis0themarketpositionwasshort.)

Returns the direction and the number of contracts traded in the open trade. If the value is > 0, the market position was long. If the value is < 0, the market position was short.
Syntax
strategy.opentrades.size(trade_num) → series float
Arguments
trade_num (series int) The trade number of the open trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
strategy("`strategy.opentrades.size` Example 1")

## [// We calculate the max amt of shares we can buy.](https://www.tradingview.com/pine-script-reference/v6/#var_Wecalculatethemaxamtofshareswecanbuy.)

// We calculate the max amt of shares we can buy.
amtShares = math.floor(strategy.equity / close)
// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long, qty = amtShares)
if bar_index % 20 == 0
    strategy.close("Long")

## [// Plot the number of contracts in the latest open trade.](https://www.tradingview.com/pine-script-reference/v6/#var_Plotthenumberofcontractsinthelatestopentrade.)

// Plot the number of contracts in the latest open trade.
plot(strategy.opentrades.size(strategy.opentrades - 1), "Amount of contracts in latest open trade")
Example

## [// Calculates the average profit percentage for all open trades.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatestheaverageprofitpercentageforallopentrades.)

// Calculates the average profit percentage for all open trades.
//@version=6
strategy("`strategy.opentrades.size` Example 2")

## [// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.](https://www.tradingview.com/pine-script-reference/v6/#var_Strategycallstoenterlongtradesevery15barsandexitlongtradesevery20bars.)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

## [// Calculate profit for all open trades.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculateprofitforallopentrades.)

// Calculate profit for all open trades.
profitPct = 0.0
for tradeNo = 0 to strategy.opentrades - 1
    entryP = strategy.opentrades.entry_price(tradeNo)
    exitP = close
    profitPct += (exitP - entryP) / entryP *strategy.opentrades.size(tradeNo)* 100

## [// Calculate average profit percent for all open trades.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculateaverageprofitpercentforallopentrades.)

// Calculate average profit percent for all open trades.
avgProfitPct = nz(profitPct / strategy.opentrades)
plot(avgProfitPct)
See also
strategy.closedtrades.size()
strategy.position_size
strategy.opentrades
strategy.closedtrades
strategy.order()

## [Creates a new order to open, add to, or exit from a position. If an unfilled order with the same id exists, a call to this command modifies that order.](https://www.tradingview.com/pine-script-reference/v6/#var_Createsanewordertoopenaddtoorexitfromaposition.Ifanunfilledorderwiththesameidexistsacalltothiscommandmodifiesthatorder.)

Creates a new order to open, add to, or exit from a position. If an unfilled order with the same id exists, a call to this command modifies that order.
The resulting order's type depends on the limit and stop parameters. If the call does not contain limit or stop arguments, it creates a market order that executes on the next tick. If the call specifies a limit value but no stop value, it places a limit order that executes after the market price reaches the limit value or a better price (lower for buy orders and higher for sell orders). If the call specifies a stop value but no limit value, it places a stop order that executes after the market price reaches the stop value or a worse price (higher for buy orders and lower for sell orders). If the call contains limit and stop arguments, it creates a stop-limit order, which generates a limit order at the limit price only after the market price reaches the stop value or a worse price.
Orders from this command, unlike those from strategy.entry(), are not affected by the pyramiding parameter of the strategy() declaration statement. Strategies can open any number of trades in the same direction with calls to this function.
This command does not automatically reverse open positions because it does not exclusively create entry orders like strategy.entry() does. For example, if there is an open long position of five shares, an order from this command with a qty of 5 and a direction of strategy.short triggers the sale of five shares, which closes the position.
Learn more
Syntax
strategy.order(id, direction, qty, limit, stop, oca_name, oca_type, comment, alert_message, disable_alert) → void
Arguments
id (series string) The identifier of the order, which corresponds to an entry or exit ID in the strategy's trades after the order fills. If the strategy opens a new position after filling the order, the order's ID becomes the strategy.position_entry_name value. Strategy commands can reference the order ID to cancel or modify pending orders and generate exit orders for specific open trades. The Strategy Tester and the chart display the order ID unless the command specifies a comment value.
direction (series strategy_direction) The direction of the trade. Possible values: strategy.long for a long trade, strategy.short for a short one.
qty (series int/float) Optional. The number of contracts/shares/lots/units to trade when the order fills. The default is na, which means that the command uses the default_qty_type and default_qty_value parameters of the strategy() declaration statement to determine the quantity.
limit (series int/float) Optional. The limit price of the order. If specified, the command creates a limit or stop-limit order, depending on whether the stop value is also specified. The default is na, which means the resulting order is not of the limit or stop-limit type.
stop (series int/float) Optional. The stop price of the order. If specified, the command creates a stop or stop-limit order, depending on whether the limit value is also specified. The default is na, which means the resulting order is not of the stop or stop-limit type.
oca_name (series string) Optional. The name of the order's One-Cancels-All (OCA) group. When a pending order with the same oca_name and oca_type parameters executes, that order affects all unfilled orders in the group. The default is an empty string, which means the order does not belong to an OCA group.
oca_type (input string) Optional. Specifies how an unfilled order behaves when another pending order with the same oca_name and oca_type values executes. Possible values: strategy.oca.cancel, strategy.oca.reduce, strategy.oca.none. The default is strategy.oca.none.
comment (series string) Optional. Additional notes on the filled order. If the value is not an empty string, the Strategy Tester and the chart show this text for the order instead of the specified id. The default is an empty string.
alert_message (series string) Optional. Custom text for the alert that fires when an order fills. If the "Message" field of the "Create Alert" dialog box contains the {{strategy.order.alert_message}} placeholder, the alert message replaces the placeholder with this text. The default is an empty string.
disable_alert (series bool) Optional. If true when the command creates an order, the strategy does not trigger an alert when that order fills. This parameter accepts a "series" value, meaning users can control which orders trigger alerts when they execute. The default is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
strategy("Market order strategy", overlay = true)

## [// Calculate a 14-bar and 28-bar moving average of `close` prices.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatea14-barand28-barmovingaverageofcloseprices.)

// Calculate a 14-bar and 28-bar moving average of `close` prices.
float sma14 = ta.sma(close, 14)
float sma28 = ta.sma(close, 28)

## [// Place a market order to enter a long position when `sma14` crosses over `sma28`.](https://www.tradingview.com/pine-script-reference/v6/#var_Placeamarketordertoenteralongpositionwhensma14crossesoversma28.)

// Place a market order to enter a long position when `sma14` crosses over `sma28`.
if ta.crossover(sma14, sma28) and strategy.position_size == 0
    strategy.order("My Long Entry ID", strategy.long)

## [// Place a market order to sell the same quantity as the long trade when `sma14` crosses under `sma28`,](https://www.tradingview.com/pine-script-reference/v6/#var_Placeamarketordertosellthesamequantityasthelongtradewhensma14crossesundersma28)

// Place a market order to sell the same quantity as the long trade when `sma14` crosses under `sma28`,
// effectively closing the long position.
if ta.crossunder(sma14, sma28) and strategy.position_size > 0
    strategy.order("My Long Exit ID", strategy.short)
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
strategy("Limit and stop exit strategy", overlay = true)

## [//@variable The distance from the long entry price for each short limit order.](https://www.tradingview.com/pine-script-reference/v6/#var_variableThedistancefromthelongentrypriceforeachshortlimitorder.)

//@variable The distance from the long entry price for each short limit order.
float shortOffsetInput = input.int(200, "Sell limit/stop offset, in ticks", 1) * syminfo.mintick

## [//@function Draws a label and line at the specified `price` to visualize a limit order's level.](https://www.tradingview.com/pine-script-reference/v6/#var_functionDrawsalabelandlineatthespecifiedpricetovisualizealimitorderslevel.)

//@function Draws a label and line at the specified `price` to visualize a limit order's level.
drawLimit(float price, bool isLong, bool isStop = false) =>
    color col = isLong ? color.blue : color.red
    label.new(
         bar_index, price, (isLong ? "Long " : "Short ") + (isStop ? "stop" : "limit") + " order created",
         style = label.style_label_right, color = col, textcolor = color.white
     )
    line.new(bar_index, price, bar_index + 1, price, extend = extend.right, style = line.style_dashed, color = col)

## [//@function Stops the `l` line from extending further.](https://www.tradingview.com/pine-script-reference/v6/#var_functionStopsthellinefromextendingfurther.)

//@function Stops the `l` line from extending further.
method stopExtend(line l) =>
    l.set_x2(bar_index)
    l.set_extend(extend.none)

## [// Initialize two `line` variables to reference limit and stop line IDs.](https://www.tradingview.com/pine-script-reference/v6/#var_InitializetwolinevariablestoreferencelimitandstoplineIDs.)

// Initialize two `line` variables to reference limit and stop line IDs.
var line profitLimit = na
var line lossStop    = na

## [// Calculate a 14-bar and 28-bar moving average of `close` prices.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatea14-barand28-barmovingaverageofcloseprices.)

// Calculate a 14-bar and 28-bar moving average of `close` prices.
float sma14 = ta.sma(close, 14)
float sma28 = ta.sma(close, 28)

## [if ta.crossover(sma14, sma28) and strategy.position_size == 0](https://www.tradingview.com/pine-script-reference/v6/#var_ifta.crossoversma14sma28andstrategy.position_size0)

if ta.crossover(sma14, sma28) and strategy.position_size == 0
    // Place a market order to enter a long position.
    strategy.order("My Long Entry ID", strategy.long)

## [if strategy.position_size > 0 and strategy.position_size[1] == 0](https://www.tradingview.com/pine-script-reference/v6/#var_ifstrategy.position_size0andstrategy.position_size10)

if strategy.position_size > 0 and strategy.position_size[1] == 0
    //@variable The entry price of the long trade.
    float entryPrice = strategy.opentrades.entry_price(0)
    // Calculate short limit and stop levels above and below the `entryPrice`.
    float profitLevel = entryPrice + shortOffsetInput
    float lossLevel   = entryPrice - shortOffsetInput
    // Place short limit and stop orders at the `profitLevel` and `lossLevel`.
    strategy.order("Profit", strategy.short, limit = profitLevel, oca_name = "Bracket", oca_type = strategy.oca.cancel)
    strategy.order("Loss", strategy.short, stop = lossLevel, oca_name = "Bracket", oca_type = strategy.oca.cancel)
    // Make new drawings for the `profitLimit` and `lossStop` lines.
    profitLimit := drawLimit(profitLevel, isLong = false)
    lossStop    := drawLimit(lossLevel, isLong = false, isStop = true)

## [if ta.change(strategy.closedtrades) > 0](https://www.tradingview.com/pine-script-reference/v6/#var_ifta.changestrategy.closedtrades0)

if ta.change(strategy.closedtrades) > 0
    // Stop extending the `profitLimit` and `lossStop` lines.
    profitLimit.stopExtend()
    lossStop.stopExtend()
strategy.risk.allow_entry_in()

## [This function can be used to specify in which market direction the strategy.entry() function is allowed to open positions.](https://www.tradingview.com/pine-script-reference/v6/#var_Thisfunctioncanbeusedtospecifyinwhichmarketdirectionthestrategy.entryfunctionisallowedtoopenpositions.)

This function can be used to specify in which market direction the strategy.entry() function is allowed to open positions.
Syntax
strategy.risk.allow_entry_in(value) → void
Arguments
value (simple string) The allowed direction. Possible values: strategy.direction.all, strategy.direction.long, strategy.direction.short
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
strategy("strategy.risk.allow_entry_in")

## [strategy.risk.allow_entry_in(strategy.direction.long)](https://www.tradingview.com/pine-script-reference/v6/#var_strategy.risk.allow_entry_instrategy.direction.long)

strategy.risk.allow_entry_in(strategy.direction.long)
if open > close
    strategy.entry("Long", strategy.long)
// Instead of opening a short position with 10 contracts, this command will close long entries.
if open < close
    strategy.entry("Short", strategy.short, qty = 10)
strategy.risk.max_cons_loss_days()

## [The purpose of this rule is to cancel all pending orders, close all open positions and stop placing orders after a specified number of consecutive days with losses. The rule affects the whole strategy.](https://www.tradingview.com/pine-script-reference/v6/#var_Thepurposeofthisruleistocancelallpendingorderscloseallopenpositionsandstopplacingordersafteraspecifiednumberofconsecutivedayswithlosses.Theruleaffectsthewholestrategy.)

The purpose of this rule is to cancel all pending orders, close all open positions and stop placing orders after a specified number of consecutive days with losses. The rule affects the whole strategy.
Syntax
strategy.risk.max_cons_loss_days(count, alert_message) → void
Arguments
count (simple int) A required parameter. The allowed number of consecutive days with losses.
alert_message (simple string) An optional parameter which replaces the {{strategy.order.alert_message}} placeholder when it is used in the "Create Alert" dialog box's "Message" field.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
strategy("risk.max_cons_loss_days Demo 1")
strategy.risk.max_cons_loss_days(3) // No orders will be placed after 3 days, if each day is with loss.
plot(strategy.position_size)
strategy.risk.max_drawdown()

## [The purpose of this rule is to determine maximum drawdown. The rule affects the whole strategy. Once the maximum drawdown value is reached, all pending orders are cancelled, all open positions are closed and no new orders can be placed.](https://www.tradingview.com/pine-script-reference/v6/#var_Thepurposeofthisruleistodeterminemaximumdrawdown.Theruleaffectsthewholestrategy.Oncethemaximumdrawdownvalueisreachedallpendingordersarecancelledallopenpositionsareclosedandnoneworderscanbeplaced.)

The purpose of this rule is to determine maximum drawdown. The rule affects the whole strategy. Once the maximum drawdown value is reached, all pending orders are cancelled, all open positions are closed and no new orders can be placed.
Syntax
strategy.risk.max_drawdown(value, type, alert_message) → void
Arguments
value (simple int/float) A required parameter. The maximum drawdown value. It is specified either in money (base currency), or in percentage of maximum equity. For % of equity the range of allowed values is from 0 to 100.
type (simple string) A required parameter. The type of the value. Please specify one of the following values: strategy.percent_of_equity or strategy.cash. Note: if equity drops down to zero or to a negative and the 'strategy.percent_of_equity' is specified, all pending orders are cancelled, all open positions are closed and no new orders can be placed for good.
alert_message (simple string) An optional parameter which replaces the {{strategy.order.alert_message}} placeholder when it is used in the "Create Alert" dialog box's "Message" field.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
strategy("risk.max_drawdown Demo 1")
strategy.risk.max_drawdown(50, strategy.percent_of_equity) // set maximum drawdown to 50% of maximum equity
plot(strategy.position_size)
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
strategy("risk.max_drawdown Demo 2", currency = "EUR")
strategy.risk.max_drawdown(2000, strategy.cash) // set maximum drawdown to 2000 EUR from maximum equity
plot(strategy.position_size)
strategy.risk.max_intraday_filled_orders()

## [The purpose of this rule is to determine maximum number of filled orders per 1 day (per 1 bar, if chart resolution is higher than 1 day). The rule affects the whole strategy. Once the maximum number of filled orders is reached, all pending orders are cancelled, all open positions are closed and no new orders can be placed till the end of the current trading session.](https://www.tradingview.com/pine-script-reference/v6/#var_Thepurposeofthisruleistodeterminemaximumnumberoffilledordersper1dayper1barifchartresolutionishigherthan1day.Theruleaffectsthewholestrategy.Oncethemaximumnumberoffilledordersisreachedallpendingordersarecancelledallopenpositionsareclosedandnoneworderscanbeplacedtilltheendofthecurrenttradingsession.)

The purpose of this rule is to determine maximum number of filled orders per 1 day (per 1 bar, if chart resolution is higher than 1 day). The rule affects the whole strategy. Once the maximum number of filled orders is reached, all pending orders are cancelled, all open positions are closed and no new orders can be placed till the end of the current trading session.
Syntax
strategy.risk.max_intraday_filled_orders(count, alert_message) → void
Arguments
count (simple int) A required parameter. The maximum number of filled orders per 1 day.
alert_message (simple string) An optional parameter which replaces the {{strategy.order.alert_message}} placeholder when it is used in the "Create Alert" dialog box's "Message" field.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
strategy("risk.max_intraday_filled_orders Demo")
strategy.risk.max_intraday_filled_orders(10) // After 10 orders are filled, no more strategy orders will be placed (except for a market order to exit current open market position, if there is any).
if open > close
    strategy.entry("buy", strategy.long)
if open < close
    strategy.entry("sell", strategy.short)
strategy.risk.max_intraday_loss()

## [The maximum loss value allowed during a day. It is specified either in money (base currency), or in percentage of maximum intraday equity (0 -100).](https://www.tradingview.com/pine-script-reference/v6/#var_Themaximumlossvalueallowedduringaday.Itisspecifiedeitherinmoneybasecurrencyorinpercentageofmaximumintradayequity0-100.)

The maximum loss value allowed during a day. It is specified either in money (base currency), or in percentage of maximum intraday equity (0 -100).
Syntax
strategy.risk.max_intraday_loss(value, type, alert_message) → void
Arguments
value (simple int/float) A required parameter. The maximum loss value. It is specified either in money (base currency), or in percentage of maximum intraday equity. For % of equity the range of allowed values is from 0 to 100.
type (simple string) A required parameter. The type of the value. Please specify one of the following values: strategy.percent_of_equity or strategy.cash. Note: if equity drops down to zero or to a negative and the strategy.percent_of_equity is specified, all pending orders are cancelled, all open positions are closed and no new orders can be placed for good.
alert_message (simple string) An optional parameter which replaces the {{strategy.order.alert_message}} placeholder when it is used in the "Create Alert" dialog box's "Message" field.
Example

## [// Sets the maximum intraday loss using the strategy's equity value.](https://www.tradingview.com/pine-script-reference/v6/#var_Setsthemaximumintradaylossusingthestrategysequityvalue.)

// Sets the maximum intraday loss using the strategy's equity value.
//@version=6
strategy("strategy.risk.max_intraday_loss Example 1", overlay = false, default_qty_type = strategy.percent_of_equity, default_qty_value = 100)

## [// Input for maximum intraday loss %.](https://www.tradingview.com/pine-script-reference/v6/#var_Inputformaximumintradayloss.)

// Input for maximum intraday loss %.
lossPct = input.float(10)

## [// Set maximum intraday loss to our lossPct input](https://www.tradingview.com/pine-script-reference/v6/#var_SetmaximumintradaylosstoourlossPctinput)

// Set maximum intraday loss to our lossPct input
strategy.risk.max_intraday_loss(lossPct, strategy.percent_of_equity)

## [// Enter Short at bar_index zero.](https://www.tradingview.com/pine-script-reference/v6/#var_EnterShortatbar_indexzero.)

// Enter Short at bar_index zero.
if bar_index == 0
    strategy.entry("Short", strategy.short)

## [// Store equity value from the beginning of the day](https://www.tradingview.com/pine-script-reference/v6/#var_Storeequityvaluefromthebeginningoftheday)

// Store equity value from the beginning of the day
eqFromDayStart = ta.valuewhen(ta.change(dayofweek) > 0, strategy.equity, 0)

## [// Calculate change of the current equity from the beginning of the current day.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatechangeofthecurrentequityfromthebeginningofthecurrentday.)

// Calculate change of the current equity from the beginning of the current day.
eqChgPct = 100 * ((strategy.equity - eqFromDayStart) / strategy.equity)

## [// Plot it](https://www.tradingview.com/pine-script-reference/v6/#var_Plotit)

// Plot it
plot(eqChgPct)
hline(-lossPct)
Example

## [// Sets the maximum intraday loss using the strategy's cash value.](https://www.tradingview.com/pine-script-reference/v6/#var_Setsthemaximumintradaylossusingthestrategyscashvalue.)

// Sets the maximum intraday loss using the strategy's cash value.
//@version=6
strategy("strategy.risk.max_intraday_loss Example 2", overlay = false)

## [// Input for maximum intraday loss in absolute cash value of the symbol.](https://www.tradingview.com/pine-script-reference/v6/#var_Inputformaximumintradaylossinabsolutecashvalueofthesymbol.)

// Input for maximum intraday loss in absolute cash value of the symbol.
absCashLoss = input.float(5)

## [// Set maximum intraday loss to `absCashLoss` in account currency.](https://www.tradingview.com/pine-script-reference/v6/#var_SetmaximumintradaylosstoabsCashLossinaccountcurrency.)

// Set maximum intraday loss to `absCashLoss` in account currency.
strategy.risk.max_intraday_loss(absCashLoss, strategy.cash)

## [// Enter Short at bar_index zero.](https://www.tradingview.com/pine-script-reference/v6/#var_EnterShortatbar_indexzero.)

// Enter Short at bar_index zero.
if bar_index == 0
    strategy.entry("Short", strategy.short)

## [// Store the open price value from the beginning of the day.](https://www.tradingview.com/pine-script-reference/v6/#var_Storetheopenpricevaluefromthebeginningoftheday.)

// Store the open price value from the beginning of the day.
beginPrice = ta.valuewhen(ta.change(dayofweek) > 0, open, 0)

## [// Calculate the absolute price change for the current period.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatetheabsolutepricechangeforthecurrentperiod.)

// Calculate the absolute price change for the current period.
priceChg = (close - beginPrice)

## [hline(absCashLoss)](https://www.tradingview.com/pine-script-reference/v6/#var_hlineabsCashLoss)

hline(absCashLoss)
plot(priceChg)
See also
strategy()
strategy.percent_of_equity
strategy.cash
strategy.risk.max_position_size()

## [The purpose of this rule is to determine maximum size of a market position. The rule affects the following function: strategy.entry(). The 'entry' quantity can be reduced (if needed) to such number of contracts/shares/lots/units, so the total position size doesn't exceed the value specified in 'strategy.risk.max_position_size'. If minimum possible quantity still violates the rule, the order will not be placed.](https://www.tradingview.com/pine-script-reference/v6/#var_Thepurposeofthisruleistodeterminemaximumsizeofamarketposition.Theruleaffectsthefollowingfunctionstrategy.entry.Theentryquantitycanbereducedifneededtosuchnumberofcontractsshareslotsunitssothetotalpositionsizedoesntexceedthevaluespecifiedinstrategy.risk.max_position_size.Ifminimumpossiblequantitystillviolatestheruletheorderwillnotbeplaced.)

The purpose of this rule is to determine maximum size of a market position. The rule affects the following function: strategy.entry(). The 'entry' quantity can be reduced (if needed) to such number of contracts/shares/lots/units, so the total position size doesn't exceed the value specified in 'strategy.risk.max_position_size'. If minimum possible quantity still violates the rule, the order will not be placed.
Syntax
strategy.risk.max_position_size(contracts) → void
Arguments
contracts (simple int/float) A required parameter. Maximum number of contracts/shares/lots/units in a position.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
strategy("risk.max_position_size Demo", default_qty_value = 100)
strategy.risk.max_position_size(10)
if open > close
    strategy.entry("buy", strategy.long)
plot(strategy.position_size) // max plot value will be 10
string()4 overloads

## [Casts na to string](https://www.tradingview.com/pine-script-reference/v6/#var_Castsnatostring)

Casts na to string
Syntax & Overloads
string(x) → const string
string(x) → input string
string(x) → simple string
string(x) → series string
Arguments
x (const string) The value to convert to the specified type, usually na.
Returns
The value of the argument after casting to string.
See also
float()
int()
bool()
color()
line()
label()
syminfo.prefix()2 overloads

## [Returns exchange prefix of the symbol, e.g. "NASDAQ".](https://www.tradingview.com/pine-script-reference/v6/#var_Returnsexchangeprefixofthesymbole.g.NASDAQ.)

Returns exchange prefix of the symbol, e.g. "NASDAQ".
Syntax & Overloads
syminfo.prefix(symbol) → simple string
syminfo.prefix(symbol) → series string
Arguments
symbol (simple string) Symbol. Note that the symbol should be passed with a prefix. For example: "NASDAQ:AAPL" instead of "AAPL".
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("syminfo.prefix fun", overlay=true)
i_sym = input.symbol("NASDAQ:AAPL")
pref = syminfo.prefix(i_sym)
tick = syminfo.ticker(i_sym)
t = ticker.new(pref, tick, session.extended)
s = request.security(t, "1D", close)
plot(s)
Returns
Returns exchange prefix of the symbol, e.g. "NASDAQ".
Remarks
The result of the function is used in the ticker.new()/ticker.modify() and request.security().
See also
syminfo.tickerid
syminfo.ticker
syminfo.prefix
syminfo.ticker()
ticker.new()
syminfo.ticker()2 overloads

## [Returns symbol name without exchange prefix, e.g. "AAPL".](https://www.tradingview.com/pine-script-reference/v6/#var_Returnssymbolnamewithoutexchangeprefixe.g.AAPL.)

Returns symbol name without exchange prefix, e.g. "AAPL".
Syntax & Overloads
syminfo.ticker(symbol) → simple string
syminfo.ticker(symbol) → series string
Arguments
symbol (simple string) Symbol. Note that the symbol should be passed with a prefix. For example: "NASDAQ:AAPL" instead of "AAPL".
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("syminfo.ticker fun", overlay=true)
i_sym = input.symbol("NASDAQ:AAPL")
pref = syminfo.prefix(i_sym)
tick = syminfo.ticker(i_sym)
t = ticker.new(pref, tick, session.extended)
s = request.security(t, "1D", close)
plot(s)
Returns
Returns symbol name without exchange prefix, e.g. "AAPL".
Remarks
The result of the function is used in the ticker.new()/ticker.modify() and request.security().
See also
syminfo.tickerid
syminfo.ticker
syminfo.prefix
syminfo.prefix()
ticker.new()
ta.alma()

## [Arnaud Legoux Moving Average. It uses Gaussian distribution as weights for moving average.](https://www.tradingview.com/pine-script-reference/v6/#var_ArnaudLegouxMovingAverage.ItusesGaussiandistributionasweightsformovingaverage.)

Arnaud Legoux Moving Average. It uses Gaussian distribution as weights for moving average.
Syntax
ta.alma(series, length, offset, sigma, floor) → series float
Arguments
series (series int/float) Series of values to process.
length (series int) Number of bars (length).
offset (simple int/float) Controls tradeoff between smoothness (closer to 1) and responsiveness (closer to 0).
sigma (simple int/float) Changes the smoothness of ALMA. The larger sigma the smoother ALMA.
floor (simple bool) An optional parameter. Specifies whether the offset calculation is floored before ALMA is calculated. Default value is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.alma", overlay=true)
plot(ta.alma(close, 9, 0.85, 6))

## [// same on pine, but much less efficient](https://www.tradingview.com/pine-script-reference/v6/#var_sameonpinebutmuchlessefficient)

// same on pine, but much less efficient
pine_alma(series, windowsize, offset, sigma) =>
    m = offset *(windowsize - 1)
    //m = math.floor(offset* (windowsize - 1)) // Used as m when math.floor=true
    s = windowsize / sigma
    norm = 0.0
    sum = 0.0
    for i = 0 to windowsize - 1
        weight = math.exp(-1 *math.pow(i - m, 2) / (2* math.pow(s, 2)))
        norm := norm + weight
        sum := sum + series[windowsize - i - 1] * weight
    sum / norm
plot(pine_alma(close, 9, 0.85, 6))
Returns
Arnaud Legoux Moving Average.
Remarks
na values in the source series are included in calculations and will produce an na result.
See also
ta.sma()
ta.ema()
ta.rma()
ta.wma()
ta.vwma()
ta.swma()
ta.atr()

## [Function atr (average true range) returns the RMA of true range. True range is max(high - low, abs(high - close[1]), abs(low - close[1])).](https://www.tradingview.com/pine-script-reference/v6/#var_FunctionatraveragetruerangereturnstheRMAoftruerange.Truerangeismaxhigh-lowabshigh-close1abslow-close1.)

Function atr (average true range) returns the RMA of true range. True range is max(high - low, abs(high - close[1]), abs(low - close[1])).
Syntax
ta.atr(length) → series float
Arguments
length (simple int) Length (number of bars back).
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.atr")
plot(ta.atr(14))

## [//the same on pine](https://www.tradingview.com/pine-script-reference/v6/#var_thesameonpine)

//the same on pine
pine_atr(length) =>
    trueRange = na(high[1])? high-low : math.max(math.max(high - low, math.abs(high - close[1])), math.abs(low - close[1]))
    //true range can be also calculated with ta.tr(true)
    ta.rma(trueRange, length)

## [plot(pine_atr(14))](https://www.tradingview.com/pine-script-reference/v6/#var_plotpine_atr14)

plot(pine_atr(14))
Returns
Average true range.
Remarks
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
See also
ta.tr()
ta.rma()
ta.barssince()

## [Counts the number of bars since the last time the condition was true.](https://www.tradingview.com/pine-script-reference/v6/#var_Countsthenumberofbarssincethelasttimetheconditionwastrue.)

Counts the number of bars since the last time the condition was true.
Syntax
ta.barssince(condition) → series int
Arguments
condition (series bool) The condition to check for.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.barssince")
// get number of bars since last color.green bar
plot(ta.barssince(close >= open))
Returns
Number of bars since condition was true.
Remarks
If the condition has never been met prior to the current bar, the function returns na.
Please note that using this variable/function can cause indicator repainting.
See also
ta.lowestbars()
ta.highestbars()
ta.valuewhen()
ta.highest()
ta.lowest()
ta.bb()

## [Bollinger Bands. A Bollinger Band is a technical analysis tool defined by a set of lines plotted two standard deviations (positively and negatively) away from a simple moving average (SMA) of the security's price, but can be adjusted to user preferences.](https://www.tradingview.com/pine-script-reference/v6/#var_BollingerBands.ABollingerBandisatechnicalanalysistooldefinedbyasetoflinesplottedtwostandarddeviationspositivelyandnegativelyawayfromasimplemovingaverageSMAofthesecurityspricebutcanbeadjustedtouserpreferences.)

Bollinger Bands. A Bollinger Band is a technical analysis tool defined by a set of lines plotted two standard deviations (positively and negatively) away from a simple moving average (SMA) of the security's price, but can be adjusted to user preferences.
Syntax
ta.bb(series, length, mult) → [series float, series float, series float]
Arguments
series (series int/float) Series of values to process.
length (series int) Number of bars (length).
mult (simple int/float) Standard deviation factor.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.bb")

## [[middle, upper, lower] = ta.bb(close, 5, 4)](https://www.tradingview.com/pine-script-reference/v6/#var_middleupperlowerta.bbclose54)

[middle, upper, lower] = ta.bb(close, 5, 4)
plot(middle, color=color.yellow)
plot(upper, color=color.yellow)
plot(lower, color=color.yellow)

## [// the same on pine](https://www.tradingview.com/pine-script-reference/v6/#var_thesameonpine)

// the same on pine
f_bb(src, length, mult) =>
    float basis = ta.sma(src, length)
    float dev = mult * ta.stdev(src, length)
    [basis, basis + dev, basis - dev]

## [[pineMiddle, pineUpper, pineLower] = f_bb(close, 5, 4)](https://www.tradingview.com/pine-script-reference/v6/#var_pineMiddlepineUpperpineLowerf_bbclose54)

[pineMiddle, pineUpper, pineLower] = f_bb(close, 5, 4)

## [plot(pineMiddle)](https://www.tradingview.com/pine-script-reference/v6/#var_plotpineMiddle)

plot(pineMiddle)
plot(pineUpper)
plot(pineLower)
Returns
Bollinger Bands.
Remarks
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
See also
ta.sma()
ta.stdev()
ta.kc()
ta.bbw()

## [Bollinger Bands Width. The Bollinger Band Width is the difference between the upper and the lower Bollinger Bands divided by the middle band.](https://www.tradingview.com/pine-script-reference/v6/#var_BollingerBandsWidth.TheBollingerBandWidthisthedifferencebetweentheupperandthelowerBollingerBandsdividedbythemiddleband.)

Bollinger Bands Width. The Bollinger Band Width is the difference between the upper and the lower Bollinger Bands divided by the middle band.
Syntax
ta.bbw(series, length, mult) → series float
Arguments
series (series int/float) Series of values to process.
length (series int) Number of bars (length).
mult (simple int/float) Standard deviation factor.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.bbw")

## [plot(ta.bbw(close, 5, 4), color=color.yellow)](https://www.tradingview.com/pine-script-reference/v6/#var_plotta.bbwclose54colorcolor.yellow)

plot(ta.bbw(close, 5, 4), color=color.yellow)

## [// the same on pine](https://www.tradingview.com/pine-script-reference/v6/#var_thesameonpine)

// the same on pine
f_bbw(src, length, mult) =>
    float basis = ta.sma(src, length)
    float dev = mult *ta.stdev(src, length)
    (((basis + dev) - (basis - dev)) / basis)* 100

## [plot(f_bbw(close, 5, 4))](https://www.tradingview.com/pine-script-reference/v6/#var_plotf_bbwclose54)

plot(f_bbw(close, 5, 4))
Returns
Bollinger Bands Width.
Remarks
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
See also
ta.bb()
ta.sma()
ta.stdev()
ta.cci()

## [The CCI (commodity channel index) is calculated as the difference between the typical price of a commodity and its simple moving average, divided by the mean absolute deviation of the typical price. The index is scaled by an inverse factor of 0.015 to provide more readable numbers.](https://www.tradingview.com/pine-script-reference/v6/#var_TheCCIcommoditychannelindexiscalculatedasthedifferencebetweenthetypicalpriceofacommodityanditssimplemovingaveragedividedbythemeanabsolutedeviationofthetypicalprice.Theindexisscaledbyaninversefactorof0.015toprovidemorereadablenumbers.)

The CCI (commodity channel index) is calculated as the difference between the typical price of a commodity and its simple moving average, divided by the mean absolute deviation of the typical price. The index is scaled by an inverse factor of 0.015 to provide more readable numbers.
Syntax
ta.cci(source, length) → series float
Arguments
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
Returns
Commodity channel index of source for length bars back.
Remarks
na values in the source series are ignored.
ta.change()3 overloads

## [Compares the current source value to its value length bars ago and returns the difference.](https://www.tradingview.com/pine-script-reference/v6/#var_Comparesthecurrentsourcevaluetoitsvaluelengthbarsagoandreturnsthedifference.)

Compares the current source value to its value length bars ago and returns the difference.
Syntax & Overloads
ta.change(source, length) → series int
ta.change(source, length) → series float
ta.change(source, length) → series bool
Arguments
source (series int) Source series.
length (series int) How far the past source value is offset from the current one, in bars. Optional. The default is 1.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator('Day and Direction Change', overlay = true)
dailyBarTime = time('1D')
isNewDay = ta.change(dailyBarTime) != 0
bgcolor(isNewDay ? color.new(color.green, 80) : na)

## [isGreenBar = close >= open](https://www.tradingview.com/pine-script-reference/v6/#var_isGreenBarcloseopen)

isGreenBar = close >= open
colorChange = ta.change(isGreenBar)
plotshape(colorChange, 'Direction Change')
Returns
The difference between the values when they are numerical. When a 'bool' source is used, returns true when the current source is different from the previous source.
Remarks
na values in the source series are included in calculations and will produce an na result.
See also
ta.mom()
ta.cross()
ta.cmo()

## [Chande Momentum Oscillator. Calculates the difference between the sum of recent gains and the sum of recent losses and then divides the result by the sum of all price movement over the same period.](https://www.tradingview.com/pine-script-reference/v6/#var_ChandeMomentumOscillator.Calculatesthedifferencebetweenthesumofrecentgainsandthesumofrecentlossesandthendividestheresultbythesumofallpricemovementoverthesameperiod.)

Chande Momentum Oscillator. Calculates the difference between the sum of recent gains and the sum of recent losses and then divides the result by the sum of all price movement over the same period.
Syntax
ta.cmo(series, length) → series float
Arguments
series (series int/float) Series of values to process.
length (series int) Number of bars (length).
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.cmo")
plot(ta.cmo(close, 5), color=color.yellow)

## [// the same on pine](https://www.tradingview.com/pine-script-reference/v6/#var_thesameonpine)

// the same on pine
f_cmo(src, length) =>
    float mom = ta.change(src)
    float sm1 = math.sum((mom >= 0) ? mom : 0.0, length)
    float sm2 = math.sum((mom >= 0) ? 0.0 : -mom, length)
    100 * (sm1 - sm2) / (sm1 + sm2)

## [plot(f_cmo(close, 5))](https://www.tradingview.com/pine-script-reference/v6/#var_plotf_cmoclose5)

plot(f_cmo(close, 5))
Returns
Chande Momentum Oscillator.
Remarks
na values in the source series are ignored.
See also
ta.rsi()
ta.stoch()
math.sum()
ta.cog()

## [The cog (center of gravity) is an indicator based on statistics and the Fibonacci golden ratio.](https://www.tradingview.com/pine-script-reference/v6/#var_ThecogcenterofgravityisanindicatorbasedonstatisticsandtheFibonaccigoldenratio.)

The cog (center of gravity) is an indicator based on statistics and the Fibonacci golden ratio.
Syntax
ta.cog(source, length) → series float
Arguments
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.cog", overlay=true)
plot(ta.cog(close, 10))

## [// the same on pine](https://www.tradingview.com/pine-script-reference/v6/#var_thesameonpine)

// the same on pine
pine_cog(source, length) =>
    sum = math.sum(source, length)
    num = 0.0
    for i = 0 to length - 1
        price = source[i]
        num := num + price * (i + 1)
    -num / sum

## [plot(pine_cog(close, 10))](https://www.tradingview.com/pine-script-reference/v6/#var_plotpine_cogclose10)

plot(pine_cog(close, 10))
Returns
Center of Gravity.
Remarks
na values in the source series are ignored.
See also
ta.stoch()
ta.correlation()

## [Correlation coefficient. Describes the degree to which two series tend to deviate from their ta.sma() values.](https://www.tradingview.com/pine-script-reference/v6/#var_Correlationcoefficient.Describesthedegreetowhichtwoseriestendtodeviatefromtheirta.smavalues.)

Correlation coefficient. Describes the degree to which two series tend to deviate from their ta.sma() values.
Syntax
ta.correlation(source1, source2, length) → series float
Arguments
source1 (series int/float) Source series.
source2 (series int/float) Target series.
length (series int) Length (number of bars back).
Returns
Correlation coefficient.
Remarks
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
See also
request.security()
ta.cross()

## [Syntax](https://www.tradingview.com/pine-script-reference/v6/#var_Syntax)

Syntax
ta.cross(source1, source2) → series bool
Arguments
source1 (series int/float) First data series.
source2 (series int/float) Second data series.
Returns
true if two series have crossed each other, otherwise false.
See also
ta.change()
ta.crossover()

## [The source1-series is defined as having crossed over source2-series if, on the current bar, the value of source1 is greater than the value of source2, and on the previous bar, the value of source1 was less than or equal to the value of source2.](https://www.tradingview.com/pine-script-reference/v6/#var_Thesource1-seriesisdefinedashavingcrossedoversource2-seriesifonthecurrentbarthevalueofsource1isgreaterthanthevalueofsource2andonthepreviousbarthevalueofsource1waslessthanorequaltothevalueofsource2.)

The source1-series is defined as having crossed over source2-series if, on the current bar, the value of source1 is greater than the value of source2, and on the previous bar, the value of source1 was less than or equal to the value of source2.
Syntax
ta.crossover(source1, source2) → series bool
Arguments
source1 (series int/float) First data series.
source2 (series int/float) Second data series.
Returns
true if source1 crossed over source2 otherwise false.
ta.crossunder()

## [The source1-series is defined as having crossed under source2-series if, on the current bar, the value of source1 is less than the value of source2, and on the previous bar, the value of source1 was greater than or equal to the value of source2.](https://www.tradingview.com/pine-script-reference/v6/#var_Thesource1-seriesisdefinedashavingcrossedundersource2-seriesifonthecurrentbarthevalueofsource1islessthanthevalueofsource2andonthepreviousbarthevalueofsource1wasgreaterthanorequaltothevalueofsource2.)

The source1-series is defined as having crossed under source2-series if, on the current bar, the value of source1 is less than the value of source2, and on the previous bar, the value of source1 was greater than or equal to the value of source2.
Syntax
ta.crossunder(source1, source2) → series bool
Arguments
source1 (series int/float) First data series.
source2 (series int/float) Second data series.
Returns
true if source1 crossed under source2 otherwise false.
ta.cum()

## [Cumulative (total) sum of source. In other words it's a sum of all elements of source.](https://www.tradingview.com/pine-script-reference/v6/#var_Cumulativetotalsumofsource.Inotherwordsitsasumofallelementsofsource.)

Cumulative (total) sum of source. In other words it's a sum of all elements of source.
Syntax
ta.cum(source) → series float
Arguments
source (series int/float) Source used for the calculation.
Returns
Total sum series.
See also
math.sum()
ta.dev()

## [Measure of difference between the series and it's ta.sma()](https://www.tradingview.com/pine-script-reference/v6/#var_Measureofdifferencebetweentheseriesanditsta.sma)

Measure of difference between the series and it's ta.sma()
Syntax
ta.dev(source, length) → series float
Arguments
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.dev")
plot(ta.dev(close, 10))

## [// the same on pine](https://www.tradingview.com/pine-script-reference/v6/#var_thesameonpine)

// the same on pine
pine_dev(source, length) =>
    mean = ta.sma(source, length)
    sum = 0.0
    for i = 0 to length - 1
        val = source[i]
        sum := sum + math.abs(val - mean)
    dev = sum/length
plot(pine_dev(close, 10))
Returns
Deviation of source for length bars back.
Remarks
na values in the source series are ignored.
See also
ta.variance()
ta.stdev()
ta.dmi()

## [The dmi function returns the directional movement index.](https://www.tradingview.com/pine-script-reference/v6/#var_Thedmifunctionreturnsthedirectionalmovementindex.)

The dmi function returns the directional movement index.
Syntax
ta.dmi(diLength, adxSmoothing) → [series float, series float, series float]
Arguments
diLength (simple int) DI Period.
adxSmoothing (simple int) ADX Smoothing Period.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator(title="Directional Movement Index", shorttitle="DMI", format=format.price, precision=4)
len = input.int(17, minval=1, title="DI Length")
lensig = input.int(14, title="ADX Smoothing", minval=1)
[diplus, diminus, adx] = ta.dmi(len, lensig)
plot(adx, color=color.red, title="ADX")
plot(diplus, color=color.blue, title="+DI")
plot(diminus, color=color.orange, title="-DI")
Returns
Tuple of three DMI series: Positive Directional Movement (+DI), Negative Directional Movement (-DI) and Average Directional Movement Index (ADX).
See also
ta.rsi()
ta.tsi()
ta.mfi()
ta.ema()

## [The ema function returns the exponentially weighted moving average. In ema weighting factors decrease exponentially. It calculates by using a formula: EMA = alpha *source + (1 - alpha)* EMA[1], where alpha = 2 / (length + 1).](https://www.tradingview.com/pine-script-reference/v6/#var_Theemafunctionreturnstheexponentiallyweightedmovingaverage.Inemaweightingfactorsdecreaseexponentially.ItcalculatesbyusingaformulaEMAalphasource1-alphaEMA1wherealpha2length1.)

The ema function returns the exponentially weighted moving average. In ema weighting factors decrease exponentially. It calculates by using a formula: EMA = alpha *source + (1 - alpha)* EMA[1], where alpha = 2 / (length + 1).
Syntax
ta.ema(source, length) → series float
Arguments
source (series int/float) Series of values to process.
length (simple int) Number of bars (length).
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.ema")
plot(ta.ema(close, 15))

## [//the same on pine](https://www.tradingview.com/pine-script-reference/v6/#var_thesameonpine)

//the same on pine
pine_ema(src, length) =>
    alpha = 2 / (length + 1)
    sum = 0.0
    sum := na(sum[1]) ? src : alpha *src + (1 - alpha)* nz(sum[1])
plot(pine_ema(close,15))
Returns
Exponential moving average of source with alpha = 2 / (length + 1).
Remarks
Please note that using this variable/function can cause indicator repainting.
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
See also
ta.sma()
ta.rma()
ta.wma()
ta.vwma()
ta.swma()
ta.alma()

## [ta.falling()](https://www.tradingview.com/pine-script-reference/v6/#var_ta.falling)

ta.falling()

## [Test if the source series is now falling for length bars long.](https://www.tradingview.com/pine-script-reference/v6/#var_Testifthesourceseriesisnowfallingforlengthbarslong.)

Test if the source series is now falling for length bars long.
Syntax
ta.falling(source, length) → series bool
Arguments
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
Returns
true if current source value is less than any previous source value for length bars back, false otherwise.
Remarks
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
See also
ta.rising()
ta.highest()

## [Highest value for a given number of bars back.](https://www.tradingview.com/pine-script-reference/v6/#var_Highestvalueforagivennumberofbarsback.)

Highest value for a given number of bars back.
Syntax
ta.highest(source, length) → series float
Arguments
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
Returns
Highest value in the series.
Remarks
Two args version: source is a series and length is the number of bars back.
One arg version: length is the number of bars back. Algorithm uses high as a source series.
na values in the source series are ignored.
See also
ta.lowest()
ta.lowestbars()
ta.highestbars()
ta.valuewhen()
ta.barssince()
ta.highestbars()

## [Highest value offset for a given number of bars back.](https://www.tradingview.com/pine-script-reference/v6/#var_Highestvalueoffsetforagivennumberofbarsback.)

Highest value offset for a given number of bars back.
Syntax
ta.highestbars(source, length) → series int
Arguments
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
Returns
Offset to the highest bar.
Remarks
Two args version: source is a series and length is the number of bars back.
One arg version: length is the number of bars back. Algorithm uses high as a source series.
na values in the source series are ignored.
See also
ta.lowest()
ta.highest()
ta.lowestbars()
ta.barssince()
ta.valuewhen()
ta.hma()

## [The hma function returns the Hull Moving Average.](https://www.tradingview.com/pine-script-reference/v6/#var_ThehmafunctionreturnstheHullMovingAverage.)

The hma function returns the Hull Moving Average.
Syntax
ta.hma(source, length) → series float
Arguments
source (series int/float) Series of values to process.
length (simple int) Number of bars.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("Hull Moving Average")
src = input(defval=close, title="Source")
length = input(defval=9, title="Length")
hmaBuildIn = ta.hma(src, length)
plot(hmaBuildIn, title="Hull MA", color=#674EA7)
Returns
Hull moving average of 'source' for 'length' bars back.
Remarks
na values in the source series are ignored.
See also
ta.ema()
ta.rma()
ta.wma()
ta.vwma()
ta.sma()
ta.kc()

## [Keltner Channels. Keltner channel is a technical analysis indicator showing a central moving average line plus channel lines at a distance above and below.](https://www.tradingview.com/pine-script-reference/v6/#var_KeltnerChannels.Keltnerchannelisatechnicalanalysisindicatorshowingacentralmovingaveragelinepluschannellinesatadistanceaboveandbelow.)

Keltner Channels. Keltner channel is a technical analysis indicator showing a central moving average line plus channel lines at a distance above and below.
Syntax
ta.kc(series, length, mult, useTrueRange) → [series float, series float, series float]
Arguments
series (series int/float) Series of values to process.
length (simple int) Number of bars (length).
mult (simple int/float) Standard deviation factor.
useTrueRange (simple bool) An optional parameter. Specifies if True Range is used; default is true. If the value is false, the range will be calculated with the expression (high - low).
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.kc")

## [[middle, upper, lower] = ta.kc(close, 5, 4)](https://www.tradingview.com/pine-script-reference/v6/#var_middleupperlowerta.kcclose54)

[middle, upper, lower] = ta.kc(close, 5, 4)
plot(middle, color=color.yellow)
plot(upper, color=color.yellow)
plot(lower, color=color.yellow)

## [// the same on pine](https://www.tradingview.com/pine-script-reference/v6/#var_thesameonpine)

// the same on pine
f_kc(src, length, mult, useTrueRange) =>
    float basis = ta.ema(src, length)
    float span = (useTrueRange) ? ta.tr : (high - low)
    float rangeEma = ta.ema(span, length)
    [basis, basis + rangeEma *mult, basis - rangeEma* mult]

## [[pineMiddle, pineUpper, pineLower] = f_kc(close, 5, 4, true)](https://www.tradingview.com/pine-script-reference/v6/#var_pineMiddlepineUpperpineLowerf_kcclose54true)

[pineMiddle, pineUpper, pineLower] = f_kc(close, 5, 4, true)

## [plot(pineMiddle)](https://www.tradingview.com/pine-script-reference/v6/#var_plotpineMiddle)

plot(pineMiddle)
plot(pineUpper)
plot(pineLower)
Returns
Keltner Channels.
Remarks
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
See also
ta.ema()
ta.atr()
ta.bb()
ta.kcw()

## [Keltner Channels Width. The Keltner Channels Width is the difference between the upper and the lower Keltner Channels divided by the middle channel.](https://www.tradingview.com/pine-script-reference/v6/#var_KeltnerChannelsWidth.TheKeltnerChannelsWidthisthedifferencebetweentheupperandthelowerKeltnerChannelsdividedbythemiddlechannel.)

Keltner Channels Width. The Keltner Channels Width is the difference between the upper and the lower Keltner Channels divided by the middle channel.
Syntax
ta.kcw(series, length, mult, useTrueRange) → series float
Arguments
series (series int/float) Series of values to process.
length (simple int) Number of bars (length).
mult (simple int/float) Standard deviation factor.
useTrueRange (simple bool) An optional parameter. Specifies if True Range is used; default is true. If the value is false, the range will be calculated with the expression (high - low).
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.kcw")

## [plot(ta.kcw(close, 5, 4), color=color.yellow)](https://www.tradingview.com/pine-script-reference/v6/#var_plotta.kcwclose54colorcolor.yellow)

plot(ta.kcw(close, 5, 4), color=color.yellow)

## [// the same on pine](https://www.tradingview.com/pine-script-reference/v6/#var_thesameonpine)

// the same on pine
f_kcw(src, length, mult, useTrueRange) =>
    float basis = ta.ema(src, length)
    float span = (useTrueRange) ? ta.tr : (high - low)
    float rangeEma = ta.ema(span, length)

## [((basis + rangeEma * mult) - (basis - rangeEma * mult)) / basis](https://www.tradingview.com/pine-script-reference/v6/#var_basisrangeEmamult-basis-rangeEmamultbasis)

((basis + rangeEma * mult) - (basis - rangeEma * mult)) / basis

## [plot(f_kcw(close, 5, 4, true))](https://www.tradingview.com/pine-script-reference/v6/#var_plotf_kcwclose54true)

plot(f_kcw(close, 5, 4, true))
Returns
Keltner Channels Width.
Remarks
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
See also
ta.kc()
ta.ema()
ta.atr()
ta.bb()
ta.linreg()

## [Linear regression curve. A line that best fits the prices specified over a user-defined time period. It is calculated using the least squares method. The result of this function is calculated using the formula: linreg = intercept + slope * (length - 1 - offset), where intercept and slope are the values calculated with the least squares method on source series.](https://www.tradingview.com/pine-script-reference/v6/#var_Linearregressioncurve.Alinethatbestfitsthepricesspecifiedoverauser-definedtimeperiod.Itiscalculatedusingtheleastsquaresmethod.Theresultofthisfunctioniscalculatedusingtheformulalinreginterceptslopelength-1-offsetwhereinterceptandslopearethevaluescalculatedwiththeleastsquaresmethodonsourceseries.)

Linear regression curve. A line that best fits the prices specified over a user-defined time period. It is calculated using the least squares method. The result of this function is calculated using the formula: linreg = intercept + slope * (length - 1 - offset), where intercept and slope are the values calculated with the least squares method on source series.
Syntax
ta.linreg(source, length, offset) → series float
Arguments
source (series int/float) Source series.
length (series int) Number of bars (length).
offset (simple int) Offset.
Returns
Linear regression curve.
Remarks
na values in the source series are included in calculations and will produce an na result.
ta.lowest()

## [Lowest value for a given number of bars back.](https://www.tradingview.com/pine-script-reference/v6/#var_Lowestvalueforagivennumberofbarsback.)

Lowest value for a given number of bars back.
Syntax
ta.lowest(source, length) → series float
Arguments
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
Returns
Lowest value in the series.
Remarks
Two args version: source is a series and length is the number of bars back.
One arg version: length is the number of bars back. Algorithm uses low as a source series.
na values in the source series are ignored.
See also
ta.highest()
ta.lowestbars()
ta.highestbars()
ta.valuewhen()
ta.barssince()
ta.lowestbars()

## [Lowest value offset for a given number of bars back.](https://www.tradingview.com/pine-script-reference/v6/#var_Lowestvalueoffsetforagivennumberofbarsback.)

Lowest value offset for a given number of bars back.
Syntax
ta.lowestbars(source, length) → series int
Arguments
source (series int/float) Series of values to process.
length (series int) Number of bars back.
Returns
Offset to the lowest bar.
Remarks
Two args version: source is a series and length is the number of bars back.
One arg version: length is the number of bars back. Algorithm uses low as a source series.
na values in the source series are ignored.
See also
ta.lowest()
ta.highest()
ta.highestbars()
ta.barssince()
ta.valuewhen()
ta.macd()

## [MACD (moving average convergence/divergence). It is supposed to reveal changes in the strength, direction, momentum, and duration of a trend in a stock's price.](https://www.tradingview.com/pine-script-reference/v6/#var_MACDmovingaverageconvergencedivergence.Itissupposedtorevealchangesinthestrengthdirectionmomentumanddurationofatrendinastocksprice.)

MACD (moving average convergence/divergence). It is supposed to reveal changes in the strength, direction, momentum, and duration of a trend in a stock's price.
Syntax
ta.macd(source, fastlen, slowlen, siglen) → [series float, series float, series float]
Arguments
source (series int/float) Series of values to process.
fastlen (simple int) Fast Length parameter.
slowlen (simple int) Slow Length parameter.
siglen (simple int) Signal Length parameter.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("MACD")
[macdLine, signalLine, histLine] = ta.macd(close, 12, 26, 9)
plot(macdLine, color=color.blue)
plot(signalLine, color=color.orange)
plot(histLine, color=color.red, style=plot.style_histogram)
If you need only one value, use placeholders '_' like this:
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("MACD")
[_, signalLine,_] = ta.macd(close, 12, 26, 9)
plot(signalLine, color=color.orange)
Returns
Tuple of three MACD series: MACD line, signal line and histogram line.
Remarks
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
See also
ta.sma()
ta.ema()
ta.max()

## [Returns the all-time high value of source from the beginning of the chart up to the current bar.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnstheall-timehighvalueofsourcefromthebeginningofthechartuptothecurrentbar.)

Returns the all-time high value of source from the beginning of the chart up to the current bar.
Syntax
ta.max(source) → series float
Arguments
source (series int/float) Source used for the calculation.
Remarks
na
ta.median()2 overloads

## [Returns the median of the series.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnsthemedianoftheseries.)

Returns the median of the series.
Syntax & Overloads
ta.median(source, length) → series int
ta.median(source, length) → series float
Arguments
source (series int) Series of values to process.
length (series int) Number of bars (length).
Returns
The median of the series.
Remarks
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
ta.mfi()

## [Money Flow Index. The Money Flow Index (MFI) is a technical oscillator that uses price and volume for identifying overbought or oversold conditions in an asset.](https://www.tradingview.com/pine-script-reference/v6/#var_MoneyFlowIndex.TheMoneyFlowIndexMFIisatechnicaloscillatorthatusespriceandvolumeforidentifyingoverboughtoroversoldconditionsinanasset.)

Money Flow Index. The Money Flow Index (MFI) is a technical oscillator that uses price and volume for identifying overbought or oversold conditions in an asset.
Syntax
ta.mfi(series, length) → series float
Arguments
series (series int/float) Series of values to process.
length (series int) Number of bars (length).
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("Money Flow Index")

## [plot(ta.mfi(hlc3, 14), color=color.yellow)](https://www.tradingview.com/pine-script-reference/v6/#var_plotta.mfihlc314colorcolor.yellow)

plot(ta.mfi(hlc3, 14), color=color.yellow)

## [// the same on pine](https://www.tradingview.com/pine-script-reference/v6/#var_thesameonpine)

// the same on pine
pine_mfi(src, length) =>
    float upper = math.sum(volume *(ta.change(src) <= 0.0 ? 0.0 : src), length)
    float lower = math.sum(volume* (ta.change(src) >= 0.0 ? 0.0 : src), length)
    mfi = 100.0 - (100.0 / (1.0 + upper / lower))
    mfi

## [plot(pine_mfi(hlc3, 14))](https://www.tradingview.com/pine-script-reference/v6/#var_plotpine_mfihlc314)

plot(pine_mfi(hlc3, 14))
Returns
Money Flow Index.
Remarks
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
See also
ta.rsi()
math.sum()
ta.min()

## [Returns the all-time low value of source from the beginning of the chart up to the current bar.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnstheall-timelowvalueofsourcefromthebeginningofthechartuptothecurrentbar.)

Returns the all-time low value of source from the beginning of the chart up to the current bar.
Syntax
ta.min(source) → series float
Arguments
source (series int/float) Source used for the calculation.
Remarks
na
ta.mode()2 overloads

## [Returns the mode of the series. If there are several values with the same frequency, it returns the smallest value.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnsthemodeoftheseries.Ifthereareseveralvalueswiththesamefrequencyitreturnsthesmallestvalue.)

Returns the mode of the series. If there are several values with the same frequency, it returns the smallest value.
Syntax & Overloads
ta.mode(source, length) → series int
ta.mode(source, length) → series float
Arguments
source (series int) Series of values to process.
length (series int) Number of bars (length).
Returns
The most frequently occurring value from the source. If none exists, returns the smallest value instead.
Remarks
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
ta.mom()

## [Momentum of source price and source price length bars ago. This is simply a difference: source - source[length].](https://www.tradingview.com/pine-script-reference/v6/#var_Momentumofsourcepriceandsourcepricelengthbarsago.Thisissimplyadifferencesource-sourcelength.)

Momentum of source price and source price length bars ago. This is simply a difference: source - source[length].
Syntax
ta.mom(source, length) → series float
Arguments
source (series int/float) Series of values to process.
length (series int) Offset from the current bar to the previous bar.
Returns
Momentum of source price and source price length bars ago.
Remarks
na values in the source series are included in calculations and will produce an na result.
See also
ta.change()
ta.percentile_linear_interpolation()

## [Calculates percentile using method of linear interpolation between the two nearest ranks.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatespercentileusingmethodoflinearinterpolationbetweenthetwonearestranks.)

Calculates percentile using method of linear interpolation between the two nearest ranks.
Syntax
ta.percentile_linear_interpolation(source, length, percentage) → series float
Arguments
source (series int/float) Series of values to process (source).
length (series int) Number of bars back (length).
percentage (simple int/float) Percentage, a number from range 0..100.
Returns
P-th percentile of source series for length bars back.
Remarks
Note that a percentile calculated using this method will NOT always be a member of the input data set.
na values in the source series are included in calculations and will produce an na result.
See also
ta.percentile_nearest_rank()
ta.percentile_nearest_rank()

## [Calculates percentile using method of Nearest Rank.](https://www.tradingview.com/pine-script-reference/v6/#var_CalculatespercentileusingmethodofNearestRank.)

Calculates percentile using method of Nearest Rank.
Syntax
ta.percentile_nearest_rank(source, length, percentage) → series float
Arguments
source (series int/float) Series of values to process (source).
length (series int) Number of bars back (length).
percentage (simple int/float) Percentage, a number from range 0..100.
Returns
P-th percentile of source series for length bars back.
Remarks
Using the Nearest Rank method on lengths less than 100 bars back can result in the same number being used for more than one percentile.
A percentile calculated using the Nearest Rank method will always be a member of the input data set.
The 100th percentile is defined to be the largest value in the input data set.
na values in the source series are ignored.
See also
ta.percentile_linear_interpolation()
ta.percentrank()

## [Percent rank is the percents of how many previous values was less than or equal to the current value of given series.](https://www.tradingview.com/pine-script-reference/v6/#var_Percentrankisthepercentsofhowmanypreviousvalueswaslessthanorequaltothecurrentvalueofgivenseries.)

Percent rank is the percents of how many previous values was less than or equal to the current value of given series.
Syntax
ta.percentrank(source, length) → series float
Arguments
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
Returns
Percent rank of source for length bars back.
Remarks
na values in the source series are included in calculations and will produce an na result.
ta.pivot_point_levels()

## [Calculates the pivot point levels using the specified type and anchor.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatesthepivotpointlevelsusingthespecifiedtypeandanchor.)

Calculates the pivot point levels using the specified type and anchor.
Syntax
ta.pivot_point_levels(type, anchor, developing) → array<float>
Arguments
type (series string) The type of pivot point levels. Possible values: "Traditional", "Fibonacci", "Woodie", "Classic", "DM", "Camarilla".
anchor (series bool) The condition that triggers the reset of the pivot point calculations. When true, calculations reset; when false, results calculated at the last reset persist.
developing (series bool) If false, the values are those calculated the last time the anchor condition was true. They remain constant until the anchor condition becomes true again. If true, the pivots are developing, i.e., they constantly recalculate on the data developing between the point of the last anchor (or bar zero if the anchor condition was never true) and the current bar. Optional. The default is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("Weekly Pivots", max_lines_count=500, overlay=true)
timeframe = "1W"
typeInput = input.string("Traditional", "Type", options=["Traditional", "Fibonacci", "Woodie", "Classic", "DM", "Camarilla"])
weekChange = timeframe.change(timeframe)
pivotPointsArray = ta.pivot_point_levels(typeInput, weekChange)
if weekChange
    for pivotLevel in pivotPointsArray
        line.new(time, pivotLevel, time + timeframe.in_seconds(timeframe) * 1000, pivotLevel, xloc=xloc.bar_time)
Returns
An array<float> with numerical values representing 11 pivot point levels: [P, R1, S1, R2, S2, R3, S3, R4, S4, R5, S5]. Levels absent from the specified type return na values (e.g., "DM" only calculates P, R1, and S1).
Remarks
The developing parameter cannot be true when type is set to "Woodie", because the Woodie calculation for a period depends on that period's open, which means that the pivot value is either available or unavailable, but never developing. If used together, the indicator will return a runtime error.
ta.pivothigh()2 overloads

## [This function returns price of the pivot high point. It returns 'NaN', if there was no pivot high point.](https://www.tradingview.com/pine-script-reference/v6/#var_Thisfunctionreturnspriceofthepivothighpoint.ItreturnsNaNiftherewasnopivothighpoint.)

This function returns price of the pivot high point. It returns 'NaN', if there was no pivot high point.
Syntax & Overloads
ta.pivothigh(leftbars, rightbars) → series float
ta.pivothigh(source, leftbars, rightbars) → series float
Arguments
leftbars (series int/float) Left strength.
rightbars (series int/float) Right strength.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("PivotHigh", overlay=true)
leftBars = input(2)
rightBars=input(2)
ph = ta.pivothigh(leftBars, rightBars)
plot(ph, style=plot.style_cross, linewidth=3, color= color.red, offset=-rightBars)
Returns
Price of the point or 'NaN'.
Remarks
If parameters 'leftbars' or 'rightbars' are series you should use max_bars_back() function for the 'source' variable.
ta.pivotlow()2 overloads

## [This function returns price of the pivot low point. It returns 'NaN', if there was no pivot low point.](https://www.tradingview.com/pine-script-reference/v6/#var_Thisfunctionreturnspriceofthepivotlowpoint.ItreturnsNaNiftherewasnopivotlowpoint.)

This function returns price of the pivot low point. It returns 'NaN', if there was no pivot low point.
Syntax & Overloads
ta.pivotlow(leftbars, rightbars) → series float
ta.pivotlow(source, leftbars, rightbars) → series float
Arguments
leftbars (series int/float) Left strength.
rightbars (series int/float) Right strength.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("PivotLow", overlay=true)
leftBars = input(2)
rightBars=input(2)
pl = ta.pivotlow(close, leftBars, rightBars)
plot(pl, style=plot.style_cross, linewidth=3, color= color.blue, offset=-rightBars)
Returns
Price of the point or 'NaN'.
Remarks
If parameters 'leftbars' or 'rightbars' are series you should use max_bars_back() function for the 'source' variable.
ta.range()2 overloads

## [Returns the difference between the min and max values in a series.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnsthedifferencebetweentheminandmaxvaluesinaseries.)

Returns the difference between the min and max values in a series.
Syntax & Overloads
ta.range(source, length) → series int
ta.range(source, length) → series float
Arguments
source (series int) Series of values to process.
length (series int) Number of bars (length).
Returns
The difference between the min and max values in the series.
Remarks
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
ta.rci()

## [Calculates the Rank Correlation Index (RCI), which measures the directional consistency of price movements. It evaluates the monotonic relationship between a source series and the bar index over length bars using Spearman's rank correlation coefficient. The resulting value is scaled to a range of -100 to 100, where 100 indicates the source consistently increased over the period, and -100 indicates it consistently decreased. Values between -100 and 100 reflect varying degrees of upward or downward consistency.](https://www.tradingview.com/pine-script-reference/v6/#var_CalculatestheRankCorrelationIndexRCIwhichmeasuresthedirectionalconsistencyofpricemovements.ItevaluatesthemonotonicrelationshipbetweenasourceseriesandthebarindexoverlengthbarsusingSpearmansrankcorrelationcoefficient.Theresultingvalueisscaledtoarangeof-100to100where100indicatesthesourceconsistentlyincreasedovertheperiodand-100indicatesitconsistentlydecreased.Valuesbetween-100and100reflectvaryingdegreesofupwardordownwardconsistency.)

Calculates the Rank Correlation Index (RCI), which measures the directional consistency of price movements. It evaluates the monotonic relationship between a source series and the bar index over length bars using Spearman's rank correlation coefficient. The resulting value is scaled to a range of -100 to 100, where 100 indicates the source consistently increased over the period, and -100 indicates it consistently decreased. Values between -100 and 100 reflect varying degrees of upward or downward consistency.
Syntax
ta.rci(source, length) → series float
Arguments
source (series int/float) Series of values to process.
length (simple int) Number of bars (length).
Returns
The Rank Correlation Index, a value between -100 to 100.
See also
ta.rising()

## [Test if the source series is now rising for length bars long.](https://www.tradingview.com/pine-script-reference/v6/#var_Testifthesourceseriesisnowrisingforlengthbarslong.)

Test if the source series is now rising for length bars long.
Syntax
ta.rising(source, length) → series bool
Arguments
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
Returns
true if current source is greater than any previous source for length bars back, false otherwise.
Remarks
na values in the source series are ignored.
See also
ta.falling()
ta.rma()

## [Moving average used in RSI. It is the exponentially weighted moving average with alpha = 1 / length.](https://www.tradingview.com/pine-script-reference/v6/#var_MovingaverageusedinRSI.Itistheexponentiallyweightedmovingaveragewithalpha1length.)

Moving average used in RSI. It is the exponentially weighted moving average with alpha = 1 / length.
Syntax
ta.rma(source, length) → series float
Arguments
source (series int/float) Series of values to process.
length (simple int) Number of bars (length).
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.rma")
plot(ta.rma(close, 15))

## [//the same on pine](https://www.tradingview.com/pine-script-reference/v6/#var_thesameonpine)

//the same on pine
pine_rma(src, length) =>
    alpha = 1/length
    sum = 0.0
    sum := na(sum[1]) ? ta.sma(src, length) : alpha *src + (1 - alpha)* nz(sum[1])
plot(pine_rma(close, 15))
Returns
Exponential moving average of source with alpha = 1 / length.
Remarks
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
See also
ta.sma()
ta.ema()
ta.wma()
ta.vwma()
ta.swma()
ta.alma()
ta.rsi()
ta.roc()

## [Calculates the percentage of change (rate of change) between the current value of source and its value length bars ago.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatesthepercentageofchangerateofchangebetweenthecurrentvalueofsourceanditsvaluelengthbarsago.)

Calculates the percentage of change (rate of change) between the current value of source and its value length bars ago.
It is calculated by the formula: 100 * change(src, length) / src[length].
Syntax
ta.roc(source, length) → series float
Arguments
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
Returns
The rate of change of source for length bars back.
Remarks
na values in the source series are included in calculations and will produce an na result.
ta.rsi()

## [Relative strength index. It is calculated using the ta.rma() of upward and downward changes of source over the last length bars.](https://www.tradingview.com/pine-script-reference/v6/#var_Relativestrengthindex.Itiscalculatedusingtheta.rmaofupwardanddownwardchangesofsourceoverthelastlengthbars.)

Relative strength index. It is calculated using the ta.rma() of upward and downward changes of source over the last length bars.
Syntax
ta.rsi(source, length) → series float
Arguments
source (series int/float) Series of values to process.
length (simple int) Number of bars (length).
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.rsi")
plot(ta.rsi(close, 7))

## [// same on pine, but less efficient](https://www.tradingview.com/pine-script-reference/v6/#var_sameonpinebutlessefficient)

// same on pine, but less efficient
pine_rsi(x, y) =>
    u = math.max(x - x[1], 0) // upward ta.change
    d = math.max(x[1] - x, 0) // downward ta.change
    rs = ta.rma(u, y) / ta.rma(d, y)
    res = 100 - 100 / (1 + rs)
    res

## [plot(pine_rsi(close, 7))](https://www.tradingview.com/pine-script-reference/v6/#var_plotpine_rsiclose7)

plot(pine_rsi(close, 7))
Returns
Relative strength index.
Remarks
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
See also
ta.rma()
ta.sar()

## [Parabolic SAR (parabolic stop and reverse) is a method devised by J. Welles Wilder, Jr., to find potential reversals in the market price direction of traded goods.](https://www.tradingview.com/pine-script-reference/v6/#var_ParabolicSARparabolicstopandreverseisamethoddevisedbyJ.WellesWilderJr.tofindpotentialreversalsinthemarketpricedirectionoftradedgoods.)

Parabolic SAR (parabolic stop and reverse) is a method devised by J. Welles Wilder, Jr., to find potential reversals in the market price direction of traded goods.
Syntax
ta.sar(start, inc, max) → series float
Arguments
start (simple int/float) Start.
inc (simple int/float) Increment.
max (simple int/float) Maximum.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.sar")
plot(ta.sar(0.02, 0.02, 0.2), style=plot.style_cross, linewidth=3)

## [// The same on Pine Script®](https://www.tradingview.com/pine-script-reference/v6/#var_ThesameonPineScript)

// The same on Pine Script®
pine_sar(start, inc, max) =>
    var float result = na
    var float maxMin = na
    var float acceleration = na
    var bool isBelow = false
    bool isFirstTrendBar = false

## [if bar_index == 1](https://www.tradingview.com/pine-script-reference/v6/#var_ifbar_index1)

if bar_index == 1
        if close > close[1]
            isBelow := true
            maxMin := high
            result := low[1]
        else
            isBelow := false
            maxMin := low
            result := high[1]
        isFirstTrendBar := true
        acceleration := start

## [result := result + acceleration * (maxMin - result)](https://www.tradingview.com/pine-script-reference/v6/#var_resultresultaccelerationmaxMin-result)

result := result + acceleration * (maxMin - result)

## [if isBelow](https://www.tradingview.com/pine-script-reference/v6/#var_ifisBelow)

if isBelow
        if result > low
            isFirstTrendBar := true
            isBelow := false
            result := math.max(high, maxMin)
            maxMin := low
            acceleration := start
    else
        if result < high
            isFirstTrendBar := true
            isBelow := true
            result := math.min(low, maxMin)
            maxMin := high
            acceleration := start

## [if not isFirstTrendBar](https://www.tradingview.com/pine-script-reference/v6/#var_ifnotisFirstTrendBar)

if not isFirstTrendBar
        if isBelow
            if high > maxMin
                maxMin := high
                acceleration := math.min(acceleration + inc, max)
        else
            if low < maxMin
                maxMin := low
                acceleration := math.min(acceleration + inc, max)

## [if isBelow](https://www.tradingview.com/pine-script-reference/v6/#var_ifisBelow)

if isBelow
        result := math.min(result, low[1])
        if bar_index > 1
            result := math.min(result, low[2])

## [else](https://www.tradingview.com/pine-script-reference/v6/#var_else)

else
        result := math.max(result, high[1])
        if bar_index > 1
            result := math.max(result, high[2])

## [result](https://www.tradingview.com/pine-script-reference/v6/#var_result)

result

## [plot(pine_sar(0.02, 0.02, 0.2), style=plot.style_cross, linewidth=3)](https://www.tradingview.com/pine-script-reference/v6/#var_plotpine_sar0.020.020.2styleplot.style_crosslinewidth3)

plot(pine_sar(0.02, 0.02, 0.2), style=plot.style_cross, linewidth=3)
Returns
Parabolic SAR.
ta.sma()

## [The sma function returns the moving average, that is the sum of last y values of x, divided by y.](https://www.tradingview.com/pine-script-reference/v6/#var_Thesmafunctionreturnsthemovingaveragethatisthesumoflastyvaluesofxdividedbyy.)

The sma function returns the moving average, that is the sum of last y values of x, divided by y.
Syntax
ta.sma(source, length) → series float
Arguments
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.sma")
plot(ta.sma(close, 15))

## [// same on pine, but much less efficient](https://www.tradingview.com/pine-script-reference/v6/#var_sameonpinebutmuchlessefficient)

// same on pine, but much less efficient
pine_sma(x, y) =>
    sum = 0.0
    for i = 0 to y - 1
        sum := sum + x[i] / y
    sum
plot(pine_sma(close, 15))
Returns
Simple moving average of source for length bars back.
Remarks
na values in the source series are ignored.
See also
ta.ema()
ta.rma()
ta.wma()
ta.vwma()
ta.swma()
ta.alma()
ta.stdev()

## [Syntax](https://www.tradingview.com/pine-script-reference/v6/#var_Syntax)

Syntax
ta.stdev(source, length, biased) → series float
Arguments
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
biased (series bool) Determines which estimate should be used. Optional. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.stdev")
plot(ta.stdev(close, 5))

## [//the same on pine](https://www.tradingview.com/pine-script-reference/v6/#var_thesameonpine)

//the same on pine
isZero(val, eps) => math.abs(val) <= eps

## [SUM(fst, snd) =>](https://www.tradingview.com/pine-script-reference/v6/#var_SUMfstsnd)

SUM(fst, snd) =>
    EPS = 1e-10
    res = fst + snd
    if isZero(res, EPS)
        res := 0
    else
        if not isZero(res, 1e-4)
            res := res
        else
            15

## [pine_stdev(src, length) =>](https://www.tradingview.com/pine-script-reference/v6/#var_pine_stdevsrclength)

pine_stdev(src, length) =>
    avg = ta.sma(src, length)
    sumOfSquareDeviations = 0.0
    for i = 0 to length - 1
        sum = SUM(src[i], -avg)
        sumOfSquareDeviations := sumOfSquareDeviations + sum * sum

## [stdev = math.sqrt(sumOfSquareDeviations / length)](https://www.tradingview.com/pine-script-reference/v6/#var_stdevmath.sqrtsumOfSquareDeviationslength)

stdev = math.sqrt(sumOfSquareDeviations / length)
plot(pine_stdev(close, 5))
Returns
Standard deviation.
Remarks
If biased is true, function will calculate using a biased estimate of the entire population, if false - unbiased estimate of a sample.
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
See also
ta.dev()
ta.variance()
ta.stoch()

## [Stochastic. It is calculated by a formula: 100 * (close - lowest(low, length)) / (highest(high, length) - lowest(low, length)).](https://www.tradingview.com/pine-script-reference/v6/#var_Stochastic.Itiscalculatedbyaformula100close-lowestlowlengthhighesthighlength-lowestlowlength.)

Stochastic. It is calculated by a formula: 100 * (close - lowest(low, length)) / (highest(high, length) - lowest(low, length)).
Syntax
ta.stoch(source, high, low, length) → series float
Arguments
source (series int/float) Source series.
high (series int/float) Series of high.
low (series int/float) Series of low.
length (series int) Length (number of bars back).
Returns
Stochastic.
Remarks
na values in the source series are ignored.
See also
ta.cog()
ta.supertrend()

## [The Supertrend Indicator. The Supertrend is a trend following indicator.](https://www.tradingview.com/pine-script-reference/v6/#var_TheSupertrendIndicator.TheSupertrendisatrendfollowingindicator.)

The Supertrend Indicator. The Supertrend is a trend following indicator.
Syntax
ta.supertrend(factor, atrPeriod) → [series float, series float]
Arguments
factor (series int/float) The multiplier by which the ATR will get multiplied.
atrPeriod (simple int) Length of ATR.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("Pine Script® Supertrend")

## [[supertrend, direction] = ta.supertrend(3, 10)](https://www.tradingview.com/pine-script-reference/v6/#var_supertrenddirectionta.supertrend310)

[supertrend, direction] = ta.supertrend(3, 10)
plot(direction < 0 ? supertrend : na, "Up direction", color = color.green, style=plot.style_linebr)
plot(direction > 0 ? supertrend : na, "Down direction", color = color.red, style=plot.style_linebr)

## [// The same on Pine Script®](https://www.tradingview.com/pine-script-reference/v6/#var_ThesameonPineScript)

// The same on Pine Script®
pine_supertrend(factor, atrPeriod) =>
    src = hl2
    atr = ta.atr(atrPeriod)
    upperBand = src + factor *atr
    lowerBand = src - factor* atr
    prevLowerBand = nz(lowerBand[1])
    prevUpperBand = nz(upperBand[1])

## [lowerBand := lowerBand > prevLowerBand or close[1] < prevLowerBand ? lowerBand : prevLowerBand](https://www.tradingview.com/pine-script-reference/v6/#var_lowerBandlowerBandprevLowerBandorclose1prevLowerBandlowerBandprevLowerBand)

lowerBand := lowerBand > prevLowerBand or close[1] < prevLowerBand ? lowerBand : prevLowerBand
    upperBand := upperBand < prevUpperBand or close[1] > prevUpperBand ? upperBand : prevUpperBand
    int _direction = na
    float superTrend = na
    prevSuperTrend = superTrend[1]
    if na(atr[1])
        _direction := 1
    else if prevSuperTrend == prevUpperBand
        _direction := close > upperBand ? -1 : 1
    else
        _direction := close < lowerBand ? 1 : -1
    superTrend := _direction == -1 ? lowerBand : upperBand
    [superTrend, _direction]

## [[Pine_Supertrend, pineDirection] = pine_supertrend(3, 10)](https://www.tradingview.com/pine-script-reference/v6/#var_Pine_SupertrendpineDirectionpine_supertrend310)

[Pine_Supertrend, pineDirection] = pine_supertrend(3, 10)
plot(pineDirection < 0 ? Pine_Supertrend : na, "Up direction", color = color.green, style=plot.style_linebr)
plot(pineDirection > 0 ? Pine_Supertrend : na, "Down direction", color = color.red, style=plot.style_linebr)
Returns
Tuple of two supertrend series: supertrend line and direction of trend. Possible values are 1 (down direction) and -1 (up direction).
See also
ta.macd()
ta.swma()

## [Symmetrically weighted moving average with fixed length: 4. Weights: [1/6, 2/6, 2/6, 1/6].](https://www.tradingview.com/pine-script-reference/v6/#var_Symmetricallyweightedmovingaveragewithfixedlength4.Weights16262616.)

Symmetrically weighted moving average with fixed length: 4. Weights: [1/6, 2/6, 2/6, 1/6].
Syntax
ta.swma(source) → series float
Arguments
source (series int/float) Source series.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.swma")
plot(ta.swma(close))

## [// same on pine, but less efficient](https://www.tradingview.com/pine-script-reference/v6/#var_sameonpinebutlessefficient)

// same on pine, but less efficient
pine_swma(x) =>
    x[3] *1 / 6 + x[2]* 2 / 6 + x[1] *2 / 6 + x[0]* 1 / 6
plot(pine_swma(close))
Returns
Symmetrically weighted moving average.
Remarks
na values in the source series are included in calculations and will produce an na result.
See also
ta.sma()
ta.ema()
ta.rma()
ta.wma()
ta.vwma()
ta.alma()
ta.tr()

## [Calculates the current bar's true range. Unlike a bar's actual range (high - low), true range accounts for potential gaps by taking the maximum of the current bar's actual range and the absolute distances from the previous bar's close to the current bar's high and low. The formula is: math.max(high - low, math.abs(high - close[1]), math.abs(low - close[1])).](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatesthecurrentbarstruerange.Unlikeabarsactualrangehigh-lowtruerangeaccountsforpotentialgapsbytakingthemaximumofthecurrentbarsactualrangeandtheabsolutedistancesfromthepreviousbarsclosetothecurrentbarshighandlow.Theformulaismath.maxhigh-lowmath.abshigh-close1math.abslow-close1.)

Calculates the current bar's true range. Unlike a bar's actual range (high - low), true range accounts for potential gaps by taking the maximum of the current bar's actual range and the absolute distances from the previous bar's close to the current bar's high and low. The formula is: math.max(high - low, math.abs(high - close[1]), math.abs(low - close[1])).
Syntax
ta.tr(handle_na) → series float
Arguments
handle_na (simple bool) Defines how the function calculates the result when the previous bar's close is na. If true, the function returns the bar's high - low value. If false, it returns na.
Returns
True range. It is math.max(high - low, math.abs(high - close[1]), math.abs(low - close[1])).
Remarks
ta.tr(false) is exactly the same as ta.tr.
See also
ta.tr
ta.atr()
ta.tsi()

## [True strength index. It uses moving averages of the underlying momentum of a financial instrument.](https://www.tradingview.com/pine-script-reference/v6/#var_Truestrengthindex.Itusesmovingaveragesoftheunderlyingmomentumofafinancialinstrument.)

True strength index. It uses moving averages of the underlying momentum of a financial instrument.
Syntax
ta.tsi(source, short_length, long_length) → series float
Arguments
source (series int/float) Source series.
short_length (simple int) Short length.
long_length (simple int) Long length.
Returns
True strength index. A value in range [-1, 1].
Remarks
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
ta.valuewhen()4 overloads

## [Returns the value of the source series on the bar where the condition was true on the nth most recent occurrence.](https://www.tradingview.com/pine-script-reference/v6/#var_Returnsthevalueofthesourceseriesonthebarwheretheconditionwastrueonthenthmostrecentoccurrence.)

Returns the value of the source series on the bar where the condition was true on the nth most recent occurrence.
Syntax & Overloads
ta.valuewhen(condition, source, occurrence) → series color
ta.valuewhen(condition, source, occurrence) → series int
ta.valuewhen(condition, source, occurrence) → series float
ta.valuewhen(condition, source, occurrence) → series bool
Arguments
condition (series bool) The condition to search for.
source (series color) The value to be returned from the bar where the condition is met.
occurrence (simple int) The occurrence of the condition. The numbering starts from 0 and goes back in time, so '0' is the most recent occurrence of condition, '1' is the second most recent and so forth. Must be an integer >= 0.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.valuewhen")
slow = ta.sma(close, 7)
fast = ta.sma(close, 14)
// Get value of `close` on second most recent cross
plot(ta.valuewhen(ta.cross(slow, fast), close, 1))
Remarks
This function requires execution on every bar. It is not recommended to use it inside a for or while loop structure, where its behavior can be unexpected. Please note that using this function can cause indicator repainting.
See also
ta.lowestbars()
ta.highestbars()
ta.barssince()
ta.highest()
ta.lowest()
ta.variance()

## [Variance is the expectation of the squared deviation of a series from its mean (ta.sma()), and it informally measures how far a set of numbers are spread out from their mean.](https://www.tradingview.com/pine-script-reference/v6/#var_Varianceistheexpectationofthesquareddeviationofaseriesfromitsmeanta.smaanditinformallymeasureshowfarasetofnumbersarespreadoutfromtheirmean.)

Variance is the expectation of the squared deviation of a series from its mean (ta.sma()), and it informally measures how far a set of numbers are spread out from their mean.
Syntax
ta.variance(source, length, biased) → series float
Arguments
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
biased (series bool) Determines which estimate should be used. Optional. The default is true.
Returns
Variance of source for length bars back.
Remarks
If biased is true, function will calculate using a biased estimate of the entire population, if false - unbiased estimate of a sample.
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
See also
ta.dev()
ta.stdev()
ta.vwap()2 overloads

## [Volume weighted average price.](https://www.tradingview.com/pine-script-reference/v6/#var_Volumeweightedaverageprice.)

Volume weighted average price.
Syntax & Overloads
ta.vwap(source, anchor) → series float
ta.vwap(source, anchor, stdev_mult) → [series float, series float, series float]
Arguments
source (series int/float) Source used for the VWAP calculation.
anchor (series bool) The condition that triggers the reset of VWAP calculations. When true, calculations reset; when false, calculations proceed using the values accumulated since the previous reset. Optional. The default is equivalent to passing timeframe.change() with "1D" as its argument.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("Simple VWAP")
vwap = ta.vwap(open)
plot(vwap)
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("Advanced VWAP")
vwapAnchorInput = input.string("Daily", "Anchor", options = ["Daily", "Weekly", "Monthly"])
stdevMultiplierInput = input.float(1.0, "Standard Deviation Multiplier")
anchorTimeframe = switch vwapAnchorInput
    "Daily"   => "1D"
    "Weekly"  => "1W"
    "Monthly" => "1M"
anchor = timeframe.change(anchorTimeframe)
[vwap, upper, lower] = ta.vwap(open, anchor, stdevMultiplierInput)
plot(vwap)
plot(upper, color = color.green)
plot(lower, color = color.green)
Returns
A VWAP series, or a tuple [vwap, upper_band, lower_band] if stdev_mult is specified.
Remarks
Calculations only begin the first time the anchor condition becomes true. Until then, the function returns na.
See also
ta.vwap
ta.vwma()

## [The vwma function returns volume-weighted moving average of source for length bars back. It is the same as: sma(source * volume, length) / sma(volume, length).](https://www.tradingview.com/pine-script-reference/v6/#var_Thevwmafunctionreturnsvolume-weightedmovingaverageofsourceforlengthbarsback.Itisthesameassmasourcevolumelengthsmavolumelength.)

The vwma function returns volume-weighted moving average of source for length bars back. It is the same as: sma(source * volume, length) / sma(volume, length).
Syntax
ta.vwma(source, length) → series float
Arguments
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.vwma")
plot(ta.vwma(close, 15))

## [// same on pine, but less efficient](https://www.tradingview.com/pine-script-reference/v6/#var_sameonpinebutlessefficient)

// same on pine, but less efficient
pine_vwma(x, y) =>
    ta.sma(x * volume, y) / ta.sma(volume, y)
plot(pine_vwma(close, 15))
Returns
Volume-weighted moving average of source for length bars back.
Remarks
na values in the source series are ignored.
See also
ta.sma()
ta.ema()
ta.rma()
ta.wma()
ta.swma()
ta.alma()
ta.wma()

## [The wma function returns weighted moving average of source for length bars back. In wma weighting factors decrease in arithmetical progression.](https://www.tradingview.com/pine-script-reference/v6/#var_Thewmafunctionreturnsweightedmovingaverageofsourceforlengthbarsback.Inwmaweightingfactorsdecreaseinarithmeticalprogression.)

The wma function returns weighted moving average of source for length bars back. In wma weighting factors decrease in arithmetical progression.
Syntax
ta.wma(source, length) → series float
Arguments
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ta.wma")
plot(ta.wma(close, 15))

## [// same on pine, but much less efficient](https://www.tradingview.com/pine-script-reference/v6/#var_sameonpinebutmuchlessefficient)

// same on pine, but much less efficient
pine_wma(x, y) =>
    norm = 0.0
    sum = 0.0
    for i = 0 to y - 1
        weight = (y - i) *y
        norm := norm + weight
        sum := sum + x[i]* weight
    sum / norm
plot(pine_wma(close, 15))
Returns
Weighted moving average of source for length bars back.
Remarks
na values in the source series are ignored.
See also
ta.sma()
ta.ema()
ta.rma()
ta.vwma()
ta.swma()
ta.alma()
ta.wpr()

## [Williams %R. The oscillator shows the current closing price in relation to the high and low of the past 'length' bars.](https://www.tradingview.com/pine-script-reference/v6/#var_WilliamsR.Theoscillatorshowsthecurrentclosingpriceinrelationtothehighandlowofthepastlengthbars.)

Williams %R. The oscillator shows the current closing price in relation to the high and low of the past 'length' bars.
Syntax
ta.wpr(length) → series float
Arguments
length (series int) Number of bars.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("Williams %R", shorttitle="%R", format=format.price, precision=2)
plot(ta.wpr(14), title="%R", color=color.new(#ff6d00, 0))
Returns
Williams %R.
Remarks
na values in the source series are ignored.
See also
ta.mfi()
ta.cmo()
table()

## [Casts na to table](https://www.tradingview.com/pine-script-reference/v6/#var_Castsnatotable)

Casts na to table
Syntax
table(x) → series table
Arguments
x (series table) The value to convert to the specified type, usually na.
Returns
The value of the argument after casting to table.
See also
float()
int()
bool()
color()
string()
line()
label()
table.cell()

## [The function defines a cell in the table and sets its attributes.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctiondefinesacellinthetableandsetsitsattributes.)

The function defines a cell in the table and sets its attributes.
Syntax
table.cell(table_id, column, row, text, width, height, text_color, text_halign, text_valign, text_size, bgcolor, tooltip, text_font_family, text_formatting) → void
Arguments
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
text (series string) The text to be displayed inside the cell. Optional. The default is empty string.
width (series int/float) The width of the cell as a % of the indicator's visual space. Optional. By default, auto-adjusts the width based on the text inside the cell. Value 0 has the same effect.
height (series int/float) The height of the cell as a % of the indicator's visual space. Optional. By default, auto-adjusts the height based on the text inside of the cell. Value 0 has the same effect.
text_color (series color) The color of the text. Optional. The default is color.black.
text_halign (series string) The horizontal alignment of the cell's text. Optional. The default value is text.align_center. Possible values: text.align_left, text.align_center, text.align_right.
text_valign (series string) The vertical alignment of the cell's text. Optional. The default value is text.align_center. Possible values: text.align_top, text.align_center, text.align_bottom.
text_size (series int/string) Size of the object. The size can be any positive integer, or one of the size.*built-in constant strings. The constant strings and their equivalent integer values are: size.auto (0), size.tiny (8), size.small (10), size.normal (14), size.large (20), size.huge (36). The default value is size.normal or 14.
bgcolor (series color) The background color of the text. Optional. The default is no color.
tooltip (series string) The tooltip to be displayed inside the cell. Optional.
text_font_family (series string) The font family of the text. Optional. The default value is font.family_default. Possible values: font.family_default, font.family_monospace.
text_formatting (series text_format) The formatting of the displayed text. Formatting options support addition. For example, text.format_bold + text.format_italic will make the text both bold and italicized. Possible values: text.format_none, text.format_bold, text.format_italic. Optional. The default is text.format_none.
Remarks
This function does not create the table itself, but defines the table’s cells. To use it, you first need to create a table object with table.new().
Each table.cell() call overwrites all previously defined properties of a cell. If you call table.cell() twice in a row, e.g., the first time with text='Test Text', and the second time with text_color=color.red but without a new text argument, the default value of the 'text' being an empty string, it will overwrite 'Test Text', and your cell will display an empty string. If you want, instead, to modify any of the cell's properties, use the table.cell_set_*() functions.
A single script can only display one table in each of the possible locations. If table.cell() is used on several bars to change the same attribute of a cell (e.g. change the background color of the cell to red on the first bar, then to yellow on the second bar), only the last change will be reflected in the table, i.e., the cell’s background will be yellow. Avoid unnecessary setting of cell properties by enclosing function calls in an if barstate.islast block whenever possible, to restrict their execution to the last bar of the series.
See also
table.cell_set_bgcolor()
table.cell_set_height()
table.cell_set_text()
table.cell_set_text_formatting()
table.cell_set_text_color()
table.cell_set_text_halign()
table.cell_set_text_size()
table.cell_set_text_valign()
table.cell_set_width()
table.cell_set_tooltip()
table.cell_set_bgcolor()

## [The function sets the background color of the cell.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctionsetsthebackgroundcolorofthecell.)

The function sets the background color of the cell.
Syntax
table.cell_set_bgcolor(table_id, column, row, bgcolor) → void
Arguments
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
bgcolor (series color) The background color of the cell.
See also
table.cell_set_height()
table.cell_set_text()
table.cell_set_text_color()
table.cell_set_text_halign()
table.cell_set_text_size()
table.cell_set_text_valign()
table.cell_set_width()
table.cell_set_tooltip()
table.cell_set_height()

## [The function sets the height of cell.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctionsetstheheightofcell.)

The function sets the height of cell.
Syntax
table.cell_set_height(table_id, column, row, height) → void
Arguments
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
height (series int/float) The height of the cell as a % of the chart window. Passing 0 auto-adjusts the height based on the text inside of the cell.
See also
table.cell_set_bgcolor()
table.cell_set_text()
table.cell_set_text_color()
table.cell_set_text_halign()
table.cell_set_text_size()
table.cell_set_text_valign()
table.cell_set_width()
table.cell_set_tooltip()
table.cell_set_text()

## [The function sets the text in the specified cell.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctionsetsthetextinthespecifiedcell.)

The function sets the text in the specified cell.
Syntax
table.cell_set_text(table_id, column, row, text) → void
Arguments
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
text (series string) The text to be displayed inside the cell.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("TABLE example")
var tLog = table.new(position = position.top_left, rows = 1, columns = 2, bgcolor = color.yellow, border_width=1)
table.cell(tLog, row = 0, column = 0, text = "sometext", text_color = color.blue)
table.cell_set_text(tLog, row = 0, column = 0, text = "sometext")
See also
table.cell_set_bgcolor()
table.cell_set_height()
table.cell_set_text_color()
table.cell_set_text_halign()
table.cell_set_text_size()
table.cell_set_text_valign()
table.cell_set_width()
table.cell_set_tooltip()
table.cell_set_text_formatting()
table.cell_set_text_color()

## [The function sets the color of the text inside the cell.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctionsetsthecolorofthetextinsidethecell.)

The function sets the color of the text inside the cell.
Syntax
table.cell_set_text_color(table_id, column, row, text_color) → void
Arguments
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
text_color (series color) The color of the text.
See also
table.cell_set_bgcolor()
table.cell_set_height()
table.cell_set_text()
table.cell_set_text_halign()
table.cell_set_text_size()
table.cell_set_text_valign()
table.cell_set_width()
table.cell_set_tooltip()
table.cell_set_text_font_family()

## [The function sets the font family of the text inside the cell.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctionsetsthefontfamilyofthetextinsidethecell.)

The function sets the font family of the text inside the cell.
Syntax
table.cell_set_text_font_family(table_id, column, row, text_font_family) → void
Arguments
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
text_font_family (series string) The font family of the text. Possible values: font.family_default, font.family_monospace.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("Example of setting the table cell font")
var t = table.new(position.top_left, rows = 1, columns = 1)
table.cell(t, 0, 0, "monospace", text_color = color.blue)
table.cell_set_text_font_family(t, 0, 0, font.family_monospace)
See also
table.new()
font.family_default
font.family_monospace
table.cell_set_text_formatting()

## [Sets the formatting attributes the drawing applies to displayed text.](https://www.tradingview.com/pine-script-reference/v6/#var_Setstheformattingattributesthedrawingappliestodisplayedtext.)

Sets the formatting attributes the drawing applies to displayed text.
Syntax
table.cell_set_text_formatting(table_id, column, row, text_formatting) → void
Arguments
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
text_formatting (series text_format) The formatting of the displayed text. Formatting options support addition. For example, text.format_bold + text.format_italic will make the text both bold and italicized. Possible values: text.format_none, text.format_bold, text.format_italic. Optional. The default is text.format_none.
See also
table.cell_set_bgcolor()
table.cell_set_height()
table.cell_set_text_color()
table.cell_set_text_halign()
table.cell_set_text_size()
table.cell_set_text_valign()
table.cell_set_width()
table.cell_set_tooltip()
table.cell_set_text()
table.cell_set_text_halign()

## [The function sets the horizontal alignment of the cell's text.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctionsetsthehorizontalalignmentofthecellstext.)

The function sets the horizontal alignment of the cell's text.
Syntax
table.cell_set_text_halign(table_id, column, row, text_halign) → void
Arguments
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
text_halign (series string) The horizontal alignment of a cell's text. Possible values: text.align_left, text.align_center, text.align_right.
See also
table.cell_set_bgcolor()
table.cell_set_height()
table.cell_set_text()
table.cell_set_text_color()
table.cell_set_text_size()
table.cell_set_text_valign()
table.cell_set_width()
table.cell_set_tooltip()
table.cell_set_text_size()

## [The function sets the size of the cell's text.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctionsetsthesizeofthecellstext.)

The function sets the size of the cell's text.
Syntax
table.cell_set_text_size(table_id, column, row, text_size) → void
Arguments
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
text_size (series int/string) Size of the object. The size can be any positive integer, or one of the size.* built-in constant strings. The constant strings and their equivalent integer values are: size.auto (0), size.tiny (8), size.small (10), size.normal (14), size.large (20), size.huge (36). The default value is size.normal or 14.
See also
table.cell_set_bgcolor()
table.cell_set_height()
table.cell_set_text()
table.cell_set_text_color()
table.cell_set_text_halign()
table.cell_set_text_valign()
table.cell_set_width()
table.cell_set_tooltip()
table.cell_set_text_valign()

## [The function sets the vertical alignment of a cell's text.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctionsetstheverticalalignmentofacellstext.)

The function sets the vertical alignment of a cell's text.
Syntax
table.cell_set_text_valign(table_id, column, row, text_valign) → void
Arguments
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
text_valign (series string) The vertical alignment of the cell's text. Possible values: text.align_top, text.align_center, text.align_bottom.
See also
table.cell_set_bgcolor()
table.cell_set_height()
table.cell_set_text()
table.cell_set_text_color()
table.cell_set_text_halign()
table.cell_set_text_size()
table.cell_set_width()
table.cell_set_tooltip()
table.cell_set_tooltip()

## [The function sets the tooltip in the specified cell.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctionsetsthetooltipinthespecifiedcell.)

The function sets the tooltip in the specified cell.
Syntax
table.cell_set_tooltip(table_id, column, row, tooltip) → void
Arguments
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
tooltip (series string) The tooltip to be displayed inside the cell.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("TABLE example")
var tLog = table.new(position = position.top_left, rows = 1, columns = 2, bgcolor = color.yellow, border_width=1)
table.cell(tLog, row = 0, column = 0, text = "sometext", text_color = color.blue)
table.cell_set_tooltip(tLog, row = 0, column = 0, tooltip = "sometext")
See also
table.cell_set_bgcolor()
table.cell_set_height()
table.cell_set_text_color()
table.cell_set_text_halign()
table.cell_set_text_size()
table.cell_set_text_valign()
table.cell_set_width()
table.cell_set_text()
table.cell_set_width()

## [The function sets the width of the cell.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctionsetsthewidthofthecell.)

The function sets the width of the cell.
Syntax
table.cell_set_width(table_id, column, row, width) → void
Arguments
table_id (series table) A table object.
column (series int) The index of the cell's column. Numbering starts at 0.
row (series int) The index of the cell's row. Numbering starts at 0.
width (series int/float) The width of the cell as a % of the chart window. Passing 0 auto-adjusts the width based on the text inside of the cell.
See also
table.cell_set_bgcolor()
table.cell_set_height()
table.cell_set_text()
table.cell_set_text_color()
table.cell_set_text_halign()
table.cell_set_text_size()
table.cell_set_text_valign()
table.cell_set_tooltip()
table.clear()

## [The function removes a cell or a sequence of cells from the table. The cells are removed in a rectangle shape where the start_column and start_row specify the top-left corner, and end_column and end_row specify the bottom-right corner.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctionremovesacellorasequenceofcellsfromthetable.Thecellsareremovedinarectangleshapewherethestart_columnandstart_rowspecifythetop-leftcornerandend_columnandend_rowspecifythebottom-rightcorner.)

The function removes a cell or a sequence of cells from the table. The cells are removed in a rectangle shape where the start_column and start_row specify the top-left corner, and end_column and end_row specify the bottom-right corner.
Syntax
table.clear(table_id, start_column, start_row, end_column, end_row) → void
Arguments
table_id (series table) A table object.
start_column (series int) The index of the column of the first cell to delete. Numbering starts at 0.
start_row (series int) The index of the row of the first cell to delete. Numbering starts at 0.
end_column (series int) The index of the column of the last cell to delete. Optional. The default is the argument used for start_column. Numbering starts at 0.
end_row (series int) The index of the row of the last cell to delete. Optional. The default is the argument used for start_row. Numbering starts at 0.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("A donut", overlay=true)
if barstate.islast
    colNum = 8, rowNum = 8
    padding = "◯"
    donutTable = table.new(position.middle_right, colNum, rowNum)
    for c = 0 to colNum - 1
        for r = 0 to rowNum - 1
            table.cell(donutTable, c, r, text=padding, bgcolor=#face6e, text_color=color.new(color.black, 100))
    table.clear(donutTable, 2, 2, 5, 5)
See also
table.delete()
table.new()
table.delete()

## [The function deletes a table.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctiondeletesatable.)

The function deletes a table.
Syntax
table.delete(table_id) → void
Arguments
table_id (series table) A table object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("table.delete example")
var testTable = table.new(position = position.top_right, columns = 2, rows = 1, bgcolor = color.yellow, border_width = 1)
if barstate.islast
    table.cell(table_id = testTable, column = 0, row = 0, text = "Open is " + str.tostring(open))
    table.cell(table_id = testTable, column = 1, row = 0, text = "Close is " + str.tostring(close), bgcolor=color.teal)
if barstate.isrealtime
    table.delete(testTable)
See also
table.new()
table.clear()
table.merge_cells()

## [The function merges a sequence of cells in the table into one cell. The cells are merged in a rectangle shape where the start_column and start_row specify the top-left corner, and end_column and end_row specify the bottom-right corner.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctionmergesasequenceofcellsinthetableintoonecell.Thecellsaremergedinarectangleshapewherethestart_columnandstart_rowspecifythetop-leftcornerandend_columnandend_rowspecifythebottom-rightcorner.)

The function merges a sequence of cells in the table into one cell. The cells are merged in a rectangle shape where the start_column and start_row specify the top-left corner, and end_column and end_row specify the bottom-right corner.
Syntax
table.merge_cells(table_id, start_column, start_row, end_column, end_row) → void
Arguments
table_id (series table) A table object.
start_column (series int) The index of the column of the first cell to merge. Numbering starts at 0.
start_row (series int) The index of the row of the first cell to merge. Numbering starts at 0.
end_column (series int) The index of the column of the last cell to merge. Numbering starts at 0.
end_row (series int) The index of the row of the last cell to merge. Numbering starts at 0.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("table.merge_cells example")
SMA50  = ta.sma(close, 50)
SMA100 = ta.sma(close, 100)
SMA200 = ta.sma(close, 200)
if barstate.islast
    maTable = table.new(position.bottom_right, 3, 3, bgcolor = color.gray, border_width = 1, border_color = color.black)
    // Header
    table.cell(maTable, 0, 0, text = "SMA Table")
    table.merge_cells(maTable, 0, 0, 2, 0)
    // Cell Titles
    table.cell(maTable, 0, 1, text = "SMA 50")
    table.cell(maTable, 1, 1, text = "SMA 100")
    table.cell(maTable, 2, 1, text = "SMA 200")
    // Values
    table.cell(maTable, 0, 2, bgcolor = color.white, text = str.tostring(SMA50))
    table.cell(maTable, 1, 2, bgcolor = color.white, text = str.tostring(SMA100))
    table.cell(maTable, 2, 2, bgcolor = color.white, text = str.tostring(SMA200))
Remarks
This function will merge cells, even if their properties are not yet defined with table.cell().
The resulting merged cell inherits all of its values from the cell located at start_column:start_row, except width and height. The width and height of the resulting merged cell are based on the width/height of other cells in the neighboring columns/rows and cannot be set manually.
To modify the merged cell with any of the table.cell_set_* functions, target the cell at the start_column:start_row coordinates.
An attempt to merge a cell that has already been merged will result in an error.
See also
table.delete()
table.new()
table.new()

## [The function creates a new table.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctioncreatesanewtable.)

The function creates a new table.
Syntax
table.new(position, columns, rows, bgcolor, frame_color, frame_width, border_color, border_width, force_overlay) → series table
Arguments
position (series string) Position of the table. Possible values are: position.top_left, position.top_center, position.top_right, position.middle_left, position.middle_center, position.middle_right, position.bottom_left, position.bottom_center, position.bottom_right.
columns (series int) The number of columns in the table.
rows (series int) The number of rows in the table.
bgcolor (series color) The background color of the table. Optional. The default is no color.
frame_color (series color) The color of the outer frame of the table. Optional. The default is no color.
frame_width (series int) The width of the outer frame of the table. Optional. The default is 0.
border_color (series color) The color of the borders of the cells (excluding the outer frame). Optional. The default is no color.
border_width (series int) The width of the borders of the cells (excluding the outer frame). Optional. The default is 0.
force_overlay (const bool) If true, the drawing will display on the main chart pane, even when the script occupies a separate pane. Optional. The default is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("table.new example")
var testTable = table.new(position = position.top_right, columns = 2, rows = 1, bgcolor = color.yellow, border_width = 1)
if barstate.islast
    table.cell(table_id = testTable, column = 0, row = 0, text = "Open is " + str.tostring(open))
    table.cell(table_id = testTable, column = 1, row = 0, text = "Close is " + str.tostring(close), bgcolor=color.teal)
Returns
The ID of a table object that can be passed to other table.*() functions.
Remarks
This function creates the table object itself, but the table will not be displayed until its cells are populated. To define a cell and change its contents or attributes, use table.cell() and other table.cell_*() functions.
One table.new() call can only display one table (the last one drawn), but the function itself will be recalculated on each bar it is used on. For performance reasons, it is wise to use table.new() in conjunction with either the var keyword (so the table object is only created on the first bar) or in an if barstate.islast block (so the table object is only created on the last bar).
See also
table.cell()
table.clear()
table.delete()
table.set_bgcolor()
table.set_border_color()
table.set_border_width()
table.set_frame_color()
table.set_frame_width()
table.set_position()
table.set_bgcolor()

## [The function sets the background color of a table.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctionsetsthebackgroundcolorofatable.)

The function sets the background color of a table.
Syntax
table.set_bgcolor(table_id, bgcolor) → void
Arguments
table_id (series table) A table object.
bgcolor (series color) The background color of the table. Optional. The default is no color.
See also
table.clear()
table.delete()
table.new()
table.set_border_color()
table.set_border_width()
table.set_frame_color()
table.set_frame_width()
table.set_position()
table.set_border_color()

## [The function sets the color of the borders (excluding the outer frame) of the table's cells.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctionsetsthecolorofthebordersexcludingtheouterframeofthetablescells.)

The function sets the color of the borders (excluding the outer frame) of the table's cells.
Syntax
table.set_border_color(table_id, border_color) → void
Arguments
table_id (series table) A table object.
border_color (series color) The color of the borders. Optional. The default is no color.
See also
table.clear()
table.delete()
table.new()
table.set_frame_color()
table.set_border_width()
table.set_bgcolor()
table.set_frame_width()
table.set_position()
table.set_border_width()

## [The function sets the width of the borders (excluding the outer frame) of the table's cells.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctionsetsthewidthofthebordersexcludingtheouterframeofthetablescells.)

The function sets the width of the borders (excluding the outer frame) of the table's cells.
Syntax
table.set_border_width(table_id, border_width) → void
Arguments
table_id (series table) A table object.
border_width (series int) The width of the borders. Optional. The default is 0.
See also
table.clear()
table.delete()
table.new()
table.set_frame_color()
table.set_frame_width()
table.set_bgcolor()
table.set_border_color()
table.set_position()
table.set_frame_color()

## [The function sets the color of the outer frame of a table.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctionsetsthecoloroftheouterframeofatable.)

The function sets the color of the outer frame of a table.
Syntax
table.set_frame_color(table_id, frame_color) → void
Arguments
table_id (series table) A table object.
frame_color (series color) The color of the frame of the table. Optional. The default is no color.
See also
table.clear()
table.delete()
table.new()
table.set_border_color()
table.set_border_width()
table.set_bgcolor()
table.set_frame_width()
table.set_position()
table.set_frame_width()

## [The function set the width of the outer frame of a table.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctionsetthewidthoftheouterframeofatable.)

The function set the width of the outer frame of a table.
Syntax
table.set_frame_width(table_id, frame_width) → void
Arguments
table_id (series table) A table object.
frame_width (series int) The width of the outer frame of the table. Optional. The default is 0.
See also
table.clear()
table.delete()
table.new()
table.set_frame_color()
table.set_border_width()
table.set_bgcolor()
table.set_border_color()
table.set_position()
table.set_position()

## [The function sets the position of a table.](https://www.tradingview.com/pine-script-reference/v6/#var_Thefunctionsetsthepositionofatable.)

The function sets the position of a table.
Syntax
table.set_position(table_id, position) → void
Arguments
table_id (series table) A table object.
position (series string) Position of the table. Possible values are: position.top_left, position.top_center, position.top_right, position.middle_left, position.middle_center, position.middle_right, position.bottom_left, position.bottom_center, position.bottom_right.
See also
table.clear()
table.delete()
table.new()
table.set_bgcolor()
table.set_border_color()
table.set_border_width()
table.set_frame_color()
table.set_frame_width()
ticker.heikinashi()2 overloads

## [Creates a ticker identifier for requesting Heikin Ashi bar values.](https://www.tradingview.com/pine-script-reference/v6/#var_CreatesatickeridentifierforrequestingHeikinAshibarvalues.)

Creates a ticker identifier for requesting Heikin Ashi bar values.
Syntax & Overloads
ticker.heikinashi(symbol) → simple string
ticker.heikinashi(symbol) → series string
Arguments
symbol (simple string) Symbol ticker identifier.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ticker.heikinashi", overlay=true)
heikinashi_close = request.security(ticker.heikinashi(syminfo.tickerid), timeframe.period, close)

## [heikinashi_aapl_60_close = request.security(ticker.heikinashi("AAPL"), "60", close)](https://www.tradingview.com/pine-script-reference/v6/#var_heikinashi_aapl_60_closerequest.securityticker.heikinashiAAPL60close)

heikinashi_aapl_60_close = request.security(ticker.heikinashi("AAPL"), "60", close)
plot(heikinashi_close)
plot(heikinashi_aapl_60_close)
Returns
String value of ticker id, that can be supplied to request.security() function.
See also
syminfo.tickerid
syminfo.ticker
request.security()
ticker.renko()
ticker.linebreak()
ticker.kagi()
ticker.pointfigure()
ticker.inherit()2 overloads

## [Constructs a ticker ID for the specified symbol with additional parameters inherited from the ticker ID passed into the function call, allowing the script to request a symbol's data using the same modifiers that the from_tickerid has, including extended session, dividend adjustment, currency conversion, non-standard chart types, back-adjustment, settlement-as-close, etc.](https://www.tradingview.com/pine-script-reference/v6/#var_ConstructsatickerIDforthespecifiedsymbolwithadditionalparametersinheritedfromthetickerIDpassedintothefunctioncallallowingthescripttorequestasymbolsdatausingthesamemodifiersthatthefrom_tickeridhasincludingextendedsessiondividendadjustmentcurrencyconversionnon-standardcharttypesback-adjustmentsettlement-as-closeetc.)

Constructs a ticker ID for the specified symbol with additional parameters inherited from the ticker ID passed into the function call, allowing the script to request a symbol's data using the same modifiers that the from_tickerid has, including extended session, dividend adjustment, currency conversion, non-standard chart types, back-adjustment, settlement-as-close, etc.
Syntax & Overloads
ticker.inherit(from_tickerid, symbol) → simple string
ticker.inherit(from_tickerid, symbol) → series string
Arguments
from_tickerid (simple string) The ticker ID to inherit modifiers from.
symbol (simple string) The symbol to construct the new ticker ID for.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ticker.inherit")

## [//@variable A "NASDAQ:AAPL" ticker ID with Extender Hours enabled.](https://www.tradingview.com/pine-script-reference/v6/#var_variableANASDAQAAPLtickerIDwithExtenderHoursenabled.)

//@variable A "NASDAQ:AAPL" ticker ID with Extender Hours enabled.
tickerExtHours = ticker.new("NASDAQ", "AAPL", session.extended)
//@variable A Heikin Ashi ticker ID for "NASDAQ:AAPL" with Extended Hours enabled.
HAtickerExtHours = ticker.heikinashi(tickerExtHours)
//@variable The "NASDAQ:MSFT" symbol with no modifiers.
testSymbol = "NASDAQ:MSFT"
//@variable A ticker ID for "NASDAQ:MSFT" with inherited Heikin Ashi and Extended Hours modifiers.
testSymbolHAtickerExtHours = ticker.inherit(HAtickerExtHours, testSymbol)

## [//@variable The `close` price requested using "NASDAQ:MSFT" with inherited modifiers.](https://www.tradingview.com/pine-script-reference/v6/#var_variableTheclosepricerequestedusingNASDAQMSFTwithinheritedmodifiers.)

//@variable The `close` price requested using "NASDAQ:MSFT" with inherited modifiers.
secData = request.security(testSymbolHAtickerExtHours, "60", close, ignore_invalid_symbol = true)
//@variable The `close` price requested using "NASDAQ:MSFT" without modifiers.
compareData = request.security(testSymbol, "60", close, ignore_invalid_symbol = true)

## [plot(secData, color = color.green)](https://www.tradingview.com/pine-script-reference/v6/#var_plotsecDatacolorcolor.green)

plot(secData, color = color.green)
plot(compareData)
Remarks
If the constructed ticker ID inherits a modifier that doesn't apply to the symbol (e.g., if the from_tickerid has Extended Hours enabled, but no such option is available for the symbol), the script will ignore the modifier when requesting data using the ID.
ticker.kagi()4 overloads

## [Creates a ticker identifier for requesting Kagi values.](https://www.tradingview.com/pine-script-reference/v6/#var_CreatesatickeridentifierforrequestingKagivalues.)

Creates a ticker identifier for requesting Kagi values.
Syntax & Overloads
ticker.kagi(symbol, reversal) → simple string
ticker.kagi(symbol, reversal) → series string
ticker.kagi(symbol, param, style) → simple string
ticker.kagi(symbol, param, style) → series string
Arguments
symbol (simple string) Symbol ticker identifier.
reversal (simple int/float) Reversal amount (absolute price value).
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ticker.kagi", overlay=true)
kagi_tickerid = ticker.kagi(syminfo.tickerid, 3)
kagi_close = request.security(kagi_tickerid, timeframe.period, close)
plot(kagi_close)
Returns
String value of ticker id, that can be supplied to request.security() function.
See also
syminfo.tickerid
syminfo.ticker
request.security()
ticker.heikinashi()
ticker.renko()
ticker.linebreak()
ticker.pointfigure()
ticker.linebreak()2 overloads

## [Creates a ticker identifier for requesting Line Break values.](https://www.tradingview.com/pine-script-reference/v6/#var_CreatesatickeridentifierforrequestingLineBreakvalues.)

Creates a ticker identifier for requesting Line Break values.
Syntax & Overloads
ticker.linebreak(symbol, number_of_lines) → simple string
ticker.linebreak(symbol, number_of_lines) → series string
Arguments
symbol (simple string) Symbol ticker identifier.
number_of_lines (simple int) Number of line.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ticker.linebreak", overlay=true)
linebreak_tickerid = ticker.linebreak(syminfo.tickerid, 3)
linebreak_close = request.security(linebreak_tickerid, timeframe.period, close)
plot(linebreak_close)
Returns
String value of ticker id, that can be supplied to request.security() function.
See also
syminfo.tickerid
syminfo.ticker
request.security()
ticker.heikinashi()
ticker.renko()
ticker.kagi()
ticker.pointfigure()
ticker.modify()2 overloads

## [Creates a ticker identifier for requesting additional data for the script.](https://www.tradingview.com/pine-script-reference/v6/#var_Createsatickeridentifierforrequestingadditionaldataforthescript.)

Creates a ticker identifier for requesting additional data for the script.
Syntax & Overloads
ticker.modify(tickerid, session, adjustment, backadjustment, settlement_as_close) → simple string
ticker.modify(tickerid, session, adjustment, backadjustment, settlement_as_close) → series string
Arguments
tickerid (simple string) Symbol name with exchange prefix, e.g. 'BATS:MSFT', 'NASDAQ:MSFT' or tickerid with session and adjustment from the ticker.new() function.
session (simple string) Session type. Optional argument. Possible values: session.regular, session.extended. Session type of the current chart is syminfo.session. If session is not given, then syminfo.session value is used.
adjustment (simple string) Adjustment type. Optional argument. Possible values: adjustment.none, adjustment.splits, adjustment.dividends. If adjustment is not given, then default adjustment value is used (can be different depending on particular instrument).
backadjustment (simple backadjustment) Specifies whether past contract data on continuous futures symbols is back-adjusted. This setting only affects the data from symbols with this option available on their charts. Optional. The default is backadjustment.inherit, meaning that the modified ticker ID inherits the setting from the ticker ID passed to the tickerid parameter, or it inherits the symbol's default if the tickerid does not specify this setting. Possible values: backadjustment.inherit, backadjustment.on, backadjustment.off.
settlement_as_close (simple settlement) Specifies whether a futures symbol's close value represents the actual closing price or the settlement price on "1D" and higher timeframes. This setting only affects the data from symbols with this option available on their charts. Optional. The default is settlement_as_close.inherit, meaning that the modified ticker ID inherits the setting from the tickerid passed into the function, or it inherits the chart symbol's default if the tickerid does not specify this setting. Possible values: settlement_as_close.inherit, settlement_as_close.on, settlement_as_close.off.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ticker_modify", overlay=true)
t1 = ticker.new(syminfo.prefix, syminfo.ticker, session.regular, adjustment.splits)
c1 = request.security(t1, "D", close)
t2 = ticker.modify(t1, session.extended)
c2 = request.security(t2, "2D", close)
plot(c1)
plot(c2)
Returns
String value of ticker id, that can be supplied to request.security() function.
See also
syminfo.tickerid
syminfo.ticker
syminfo.session
session.extended
session.regular
ticker.heikinashi()
adjustment.none
adjustment.splits
adjustment.dividends
backadjustment.inherit
backadjustment.on
backadjustment.off
settlement_as_close.inherit
settlement_as_close.on
settlement_as_close.off
ticker.new()2 overloads

## [Creates a ticker identifier for requesting additional data for the script.](https://www.tradingview.com/pine-script-reference/v6/#var_Createsatickeridentifierforrequestingadditionaldataforthescript.)

Creates a ticker identifier for requesting additional data for the script.
Syntax & Overloads
ticker.new(prefix, ticker, session, adjustment, backadjustment, settlement_as_close) → simple string
ticker.new(prefix, ticker, session, adjustment, backadjustment, settlement_as_close) → series string
Arguments
prefix (simple string) Exchange prefix. For example: 'BATS', 'NYSE', 'NASDAQ'. Exchange prefix of main series is syminfo.prefix.
ticker (simple string) Ticker name. For example 'AAPL', 'MSFT', 'EURUSD'. Ticker name of the main series is syminfo.ticker.
session (simple string) Session type. Optional argument. Possible values: session.regular, session.extended. Session type of the current chart is syminfo.session. If session is not given, then syminfo.session value is used.
adjustment (simple string) Adjustment type. Optional argument. Possible values: adjustment.none, adjustment.splits, adjustment.dividends. If adjustment is not given, then default adjustment value is used (can be different depending on particular instrument).
backadjustment (simple backadjustment) Specifies whether past contract data on continuous futures symbols is back-adjusted. This setting only affects the data from symbols with this option available on their charts. Optional. The default is backadjustment.inherit, meaning that the new ticker ID inherits the symbol's default setting. Possible values: backadjustment.inherit, backadjustment.on, backadjustment.off.
settlement_as_close (simple settlement) Specifies whether a futures symbol's close value represents the actual closing price or the settlement price on "1D" and higher timeframes. This setting only affects the data from symbols with this option available on their charts. Optional. The default is settlement_as_close.inherit, meaning that the new ticker ID inherits the chart symbol's default setting. Possible values: settlement_as_close.inherit, settlement_as_close.on, settlement_as_close.off.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ticker.new", overlay=true)
t = ticker.new(syminfo.prefix, syminfo.ticker, session.regular, adjustment.splits)
t2 = ticker.heikinashi(t)
c = request.security(t2, timeframe.period, low, barmerge.gaps_on)
plot(c, style=plot.style_linebr)
Returns
String value of ticker id, that can be supplied to request.security() function.
Remarks
You may use return value of ticker.new() function as input argument for ticker.heikinashi(), ticker.renko(), ticker.linebreak(), ticker.kagi(), ticker.pointfigure() functions.
See also
syminfo.tickerid
syminfo.ticker
syminfo.session
session.extended
session.regular
ticker.heikinashi()
adjustment.none
adjustment.splits
adjustment.dividends
backadjustment.inherit
backadjustment.on
backadjustment.off
settlement_as_close.inherit
settlement_as_close.on
settlement_as_close.off
ticker.pointfigure()2 overloads

## [Creates a ticker identifier for requesting Point & Figure values.](https://www.tradingview.com/pine-script-reference/v6/#var_CreatesatickeridentifierforrequestingPointFigurevalues.)

Creates a ticker identifier for requesting Point & Figure values.
Syntax & Overloads
ticker.pointfigure(symbol, source, style, param, reversal) → simple string
ticker.pointfigure(symbol, source, style, param, reversal) → series string
Arguments
symbol (simple string) Symbol ticker identifier.
source (simple string) The source for calculating Point & Figure. Possible values are: 'hl', 'close'.
style (simple string) Specifies the ticker's box size assignment method. Possible values: "ATR" for Average True Range sizing, "Traditional" to use a fixed size, or "PercentageLTP" to use a percentage of the last trading price.
param (simple int/float) Represents the ticker's "ATR length" value if the style value is "ATR", "Box size" value if the style is "Traditional", or "Percentage" value if the style is "PercentageLTP".
reversal (simple int) Reversal amount.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ticker.pointfigure", overlay=true)
pnf_tickerid = ticker.pointfigure(syminfo.tickerid, "hl", "Traditional", 1, 3)
pnf_close = request.security(pnf_tickerid, timeframe.period, close)
plot(pnf_close)
Returns
String value of ticker id, that can be supplied to request.security() function.
See also
syminfo.tickerid
syminfo.ticker
request.security()
ticker.heikinashi()
ticker.renko()
ticker.linebreak()
ticker.kagi()
ticker.renko()2 overloads

## [Creates a ticker identifier for requesting Renko values.](https://www.tradingview.com/pine-script-reference/v6/#var_CreatesatickeridentifierforrequestingRenkovalues.)

Creates a ticker identifier for requesting Renko values.
Syntax & Overloads
ticker.renko(symbol, style, param, request_wicks, source) → simple string
ticker.renko(symbol, style, param, request_wicks, source) → series string
Arguments
symbol (simple string) Symbol ticker identifier.
style (simple string) Specifies the ticker's box size assignment method. Possible values: "ATR" for Average True Range sizing, "Traditional" to use a fixed size, or "PercentageLTP" to use a percentage of the last trading price.
param (simple int/float) Represents the ticker's "ATR length" value if the style value is "ATR", "Box size" value if the style is "Traditional", or "Percentage" value if the style is "PercentageLTP".
request_wicks (simple bool) Specifies if wick values are returned for Renko bricks. When true, high and low values requested from a symbol using the ticker formed by this function will include wick values when they are present. When false, high and low will always be equal to either open or close. Optional. The default is false. A detailed explanation of how Renko wicks are calculated can be found in our Help Center.
source (simple string) The source used to calculate bricks. Optional. Possible values: "Close", "OHLC". The default is "Close".
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ticker.renko", overlay=true)
renko_tickerid = ticker.renko(syminfo.tickerid, "ATR", 10)
renko_close = request.security(renko_tickerid, timeframe.period, close)
plot(renko_close)
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("Renko candles", overlay=false)
renko_tickerid = ticker.renko(syminfo.tickerid, "ATR", 10)
[renko_open, renko_high, renko_low, renko_close] = request.security(renko_tickerid, timeframe.period, [open, high, low, close])
plotcandle(renko_open, renko_high, renko_low, renko_close, color = renko_close > renko_open ? color.green : color.red)
Returns
String value of ticker id, that can be supplied to request.security() function.
See also
syminfo.tickerid
syminfo.ticker
request.security()
ticker.heikinashi()
ticker.linebreak()
ticker.kagi()
ticker.pointfigure()
ticker.standard()2 overloads

## [Creates a ticker to request data from a standard chart that is unaffected by modifiers like extended session, dividend adjustment, currency conversion, and the calculations of non-standard chart types: Heikin Ashi, Renko, etc. Among other things, this makes it possible to retrieve standard chart values when the script is running on a non-standard chart.](https://www.tradingview.com/pine-script-reference/v6/#var_Createsatickertorequestdatafromastandardchartthatisunaffectedbymodifierslikeextendedsessiondividendadjustmentcurrencyconversionandthecalculationsofnon-standardcharttypesHeikinAshiRenkoetc.Amongotherthingsthismakesitpossibletoretrievestandardchartvalueswhenthescriptisrunningonanon-standardchart.)

Creates a ticker to request data from a standard chart that is unaffected by modifiers like extended session, dividend adjustment, currency conversion, and the calculations of non-standard chart types: Heikin Ashi, Renko, etc. Among other things, this makes it possible to retrieve standard chart values when the script is running on a non-standard chart.
Syntax & Overloads
ticker.standard(symbol) → simple string
ticker.standard(symbol) → series string
Arguments
symbol (simple string) A ticker ID to be converted into its standard form. Optional. The default is syminfo.tickerid.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("ticker.standard", overlay = true)
// This script should be run on a non-standard chart such as HA, Renko...

## [// Requests data from the chart type the script is running on.](https://www.tradingview.com/pine-script-reference/v6/#var_Requestsdatafromthecharttypethescriptisrunningon.)

// Requests data from the chart type the script is running on.
chartTypeValue = request.security(syminfo.tickerid, "1D", close)

## [// Request data from the standard chart type, regardless of the chart type the script is running on.](https://www.tradingview.com/pine-script-reference/v6/#var_Requestdatafromthestandardcharttyperegardlessofthecharttypethescriptisrunningon.)

// Request data from the standard chart type, regardless of the chart type the script is running on.
standardChartValue = request.security(ticker.standard(syminfo.tickerid), "1D", close)

## [// This will not use a standard ticker ID because the `symbol` argument contains only the ticker — not the prefix (exchange).](https://www.tradingview.com/pine-script-reference/v6/#var_ThiswillnotuseastandardtickerIDbecausethesymbolargumentcontainsonlythetickernottheprefixexchange.)

// This will not use a standard ticker ID because the `symbol` argument contains only the ticker — not the prefix (exchange).
standardChartValue2 = request.security(ticker.standard(syminfo.ticker), "1D", close)

## [plot(chartTypeValue)](https://www.tradingview.com/pine-script-reference/v6/#var_plotchartTypeValue)

plot(chartTypeValue)
plot(standardChartValue, color = color.green)
Returns
A string representing the ticker of a standard chart in the "prefix:ticker" format. If the symbol argument does not contain the prefix and ticker information, the function returns the supplied argument as is.
See also
request.security()
time()2 overloads

## [Returns the opening UNIX timestamp for the specified timeframe and session, or na if the time point is outside the session.](https://www.tradingview.com/pine-script-reference/v6/#var_ReturnstheopeningUNIXtimestampforthespecifiedtimeframeandsessionornaifthetimepointisoutsidethesession.)

Returns the opening UNIX timestamp for the specified timeframe and session, or na if the time point is outside the session.
Syntax & Overloads
time(timeframe, session, bars_back, timeframe_bars_back) → series int
time(timeframe, session, timezone, bars_back, timeframe_bars_back) → series int
Arguments
timeframe (series string) The timeframe of the timestamp calculation. If the value is an empty string, the function uses the script's main timeframe.
session (series string) Optional. The session string for filtering times. The function returns a timestamp if the time is in the specified session, or na if the time is outside the session. If the argument is an empty string, the function uses the default, which is the symbol's session.
bars_back (series int) Optional. The bar offset on the script's main timeframe. If the value is positive, the function finds the bar that is N bars before the current bar on the main timeframe, then retrieves the timestamp of the corresponding bar on the timeframe specified by the timeframe argument. If the value is a negative number from -1 to -500, the function calculates the expected timestamp of the timeframe bar corresponding to N bars after the current bar on the main timeframe. The default is 0.
timeframe_bars_back (series int) Optional. The additional bar offset on the timeframe specified by the timeframe argument. If the value is positive, the function retrieves the timestamp of the bar that is N timeframe bars before the one corresponding to the bars_back offset. If the value is a negative number from -1 to -500, the function calculates the expected timestamp of the timeframe bar that is N timeframe bars after the one corresponding to the bars_back offset. The default is 0.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("Time", overlay=true)
// Try this on chart AAPL,1
timeinrange(res, sess) => not na(time(res, sess, "America/New_York")) ? 1 : 0
plot(timeinrange("1", "1300-1400"), color=color.red)

## [// This plots 1.0 at every start of 10 minute bar on a 1 minute chart:](https://www.tradingview.com/pine-script-reference/v6/#var_Thisplots1.0ateverystartof10minutebarona1minutechart)

// This plots 1.0 at every start of 10 minute bar on a 1 minute chart:
newbar(res) => ta.change(time(res)) == 0 ? 0 : 1
plot(newbar("10"))
While setting up a session you can specify not just the hours and minutes but also the days of the week that will be included in that session.
If the days aren't specified, the session is considered to have been set from Sunday (1) to Saturday (7), i.e. "1100-2000" is the same as "1100-1200:1234567".
You can change that by specifying the days. For example, on a symbol that is traded seven days a week with the 24-hour trading session the following script will not color Saturdays and Sundays:
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("Time", overlay=true)
t1 = time(timeframe.period, "0000-0000:23456")
bgcolor(not na(t1) ? color.new(color.blue, 90) : na)
One session argument can include several different sessions, separated by commas. For example, the following script will highlight the bars from 10:00 to 11:00 and from 14:00 to 15:00 (workdays only):
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("Time", overlay=true)
t1 = time(timeframe.period, "1000-1100,1400-1500:23456")
bgcolor(not na(t1) ? color.new(color.blue, 90) : na)
Returns
The opening UNIX timestamp.
Remarks
UNIX time is a standardized date and time representation that measures the number of non-leap seconds elapsed since January 1, 1970 at 00:00:00 UTC. Pine Script expresses UNIX time values in milliseconds. See the UNIX timestamps section of the User Manual's Time page to learn more.
See also
time
time_close()2 overloads

## [Returns the closing UNIX timestamp for the specified timeframe and session, or na if the time point is outside the session. On tick charts and price-based charts such as Renko, line break, Kagi, point & figure, and range, the function returns na on the latest realtime bar because the future closing time is unpredictable. However, it returns a valid timestamp for any previous bar.](https://www.tradingview.com/pine-script-reference/v6/#var_ReturnstheclosingUNIXtimestampforthespecifiedtimeframeandsessionornaifthetimepointisoutsidethesession.Ontickchartsandprice-basedchartssuchasRenkolinebreakKagipointfigureandrangethefunctionreturnsnaonthelatestrealtimebarbecausethefutureclosingtimeisunpredictable.Howeveritreturnsavalidtimestampforanypreviousbar.)

Returns the closing UNIX timestamp for the specified timeframe and session, or na if the time point is outside the session. On tick charts and price-based charts such as Renko, line break, Kagi, point & figure, and range, the function returns na on the latest realtime bar because the future closing time is unpredictable. However, it returns a valid timestamp for any previous bar.
Syntax & Overloads
time_close(timeframe, session, bars_back, timeframe_bars_back) → series int
time_close(timeframe, session, timezone, bars_back, timeframe_bars_back) → series int
Arguments
timeframe (series string) The timeframe of the timestamp calculation. If the value is an empty string, the function uses the script's main timeframe.
session (series string) Optional. The session string for filtering times. The function returns a timestamp if the time is in the specified session, or na if the time is outside the session. If the argument is an empty string, the function uses the default, which is the symbol's session.
bars_back (series int) Optional. The bar offset on the script's main timeframe. If the value is positive, the function finds the bar that is N bars before the current bar on the main timeframe, then retrieves the timestamp of the corresponding bar on the timeframe specified by the timeframe argument. If the value is a negative number from -1 to -500, the function calculates the expected timestamp of the timeframe bar corresponding to N bars after the current bar on the main timeframe. The default is 0.
timeframe_bars_back (series int) Optional. The additional bar offset on the timeframe specified by the timeframe argument. If the value is positive, the function retrieves the timestamp of the bar that is N timeframe bars before the one corresponding to the bars_back offset. If the value is a negative number from -1 to -500, the function calculates the expected timestamp of the timeframe bar that is N timeframe bars after the one corresponding to the bars_back offset. The default is 0.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("Time", overlay=true)
t1 = time_close(timeframe.period, "1200-1300", "America/New_York")
bgcolor(not na(t1) ? color.new(color.blue, 90) : na)
Returns
The closing UNIX timestamp.
Remarks
UNIX time is a standardized date and time representation that measures the number of non-leap seconds elapsed since January 1, 1970 at 00:00:00 UTC. Pine Script expresses UNIX time values in milliseconds. See the UNIX timestamps section of the User Manual's Time page to learn more.
See also
time_close
timeframe.change()

## [Detects changes in the specified timeframe.](https://www.tradingview.com/pine-script-reference/v6/#var_Detectschangesinthespecifiedtimeframe.)

Detects changes in the specified timeframe.
Syntax
timeframe.change(timeframe) → series bool
Arguments
timeframe (series string) String formatted according to the User manual's timeframe string specifications.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
// Run this script on an intraday chart.
indicator("New day started", overlay = true)
// Highlights the first bar of the new day.
isNewDay = timeframe.change("1D")
bgcolor(isNewDay ? color.new(color.green, 80) : na)
Returns
Returns true on the first bar of a new timeframe, false otherwise.
timeframe.from_seconds()2 overloads

## [Converts a number of seconds into a valid timeframe string.](https://www.tradingview.com/pine-script-reference/v6/#var_Convertsanumberofsecondsintoavalidtimeframestring.)

Converts a number of seconds into a valid timeframe string.
Syntax & Overloads
timeframe.from_seconds(seconds) → simple string
timeframe.from_seconds(seconds) → series string
Arguments
seconds (simple int) The number of seconds in the timeframe.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("HTF Close", "", true)
int chartTf = timeframe.in_seconds()
string tfTimes5 = timeframe.from_seconds(chartTf * 5)
float htfClose = request.security(syminfo.tickerid, tfTimes5, close)
plot(htfClose)
Returns
A timeframe string compliant with timeframe string specifications.
Remarks
If no valid timeframe exists for the quantity of seconds supplied, the next higher valid timeframe will be returned. Accordingly, one second or less will return "1S", 2-5 seconds will return "5S", and 604,799 seconds (one second less than 7 days) will return "7D".
If the seconds exactly represent two or more valid timeframes, the one with the larger base unit will be used. Thus 604,800 seconds (7 days) returns "1W", not "7D".
All values above 31,622,400 (366 days) return "12M".
See also
timeframe.in_seconds()
request.security
request.security_lower_tf
timeframe.in_seconds()2 overloads

## [Converts a timeframe string into seconds.](https://www.tradingview.com/pine-script-reference/v6/#var_Convertsatimeframestringintoseconds.)

Converts a timeframe string into seconds.
Syntax & Overloads
timeframe.in_seconds(timeframe) → simple int
timeframe.in_seconds(timeframe) → series int
Arguments
timeframe (simple string) Timeframe string in timeframe string specifications format. Optional. The default is timeframe.period.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("`timeframe_in_seconds()`"),

## [// Get a user-selected timeframe.](https://www.tradingview.com/pine-script-reference/v6/#var_Getauser-selectedtimeframe.)

// Get a user-selected timeframe.
tfInput = input.timeframe("1D")

## [// Convert it into an "int" number of seconds.](https://www.tradingview.com/pine-script-reference/v6/#var_Convertitintoanintnumberofseconds.)

// Convert it into an "int" number of seconds.
secondsInTf = timeframe.in_seconds(tfInput)

## [plot(secondsInTf)](https://www.tradingview.com/pine-script-reference/v6/#var_plotsecondsInTf)

plot(secondsInTf)
Returns
The "int" representation of the number of seconds in the timeframe string.
Remarks
When the timeframe is "1M" or more, calculations use 2628003 as the number of seconds in one month, which represents 30.4167 (365/12) days.
See also
input.timeframe()
timeframe.period
timeframe.from_seconds()
timestamp()6 overloads

## [Function timestamp returns UNIX time of specified date and time.](https://www.tradingview.com/pine-script-reference/v6/#var_FunctiontimestampreturnsUNIXtimeofspecifieddateandtime.)

Function timestamp returns UNIX time of specified date and time.
Syntax & Overloads
timestamp(dateString) → const int
timestamp(dateString) → series int
timestamp(year, month, day, hour, minute, second) → simple int
timestamp(year, month, day, hour, minute, second) → series int
timestamp(timezone, year, month, day, hour, minute, second) → simple int
timestamp(timezone, year, month, day, hour, minute, second) → series int
Arguments
dateString (const string) A string containing the date and, optionally, the time and time zone. Its format must comply with either the IETF RFC 2822 or ISO 8601 standards ("DD MMM YYYY hh:mm:ss ±hhmm" or "YYYY-MM-DDThh:mm:ss±hh:mm", so "20 Feb 2020" or "2020-02-20"). If no time is supplied, "00:00" is used. If no time zone is supplied, GMT+0 will be used. Note that this diverges from the usual behavior of the function where it returns time in the exchange's timezone.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#var_version6)

//@version=6
indicator("timestamp")
plot(timestamp(2016, 01, 19, 09, 30), linewidth=3, color=color.green)
plot(timestamp(syminfo.timezone, 2016, 01, 19, 09, 30), color=color.blue)
plot(timestamp(2016, 01, 19, 09, 30), color=color.yellow)
plot(timestamp("GMT+6", 2016, 01, 19, 09, 30))
plot(timestamp(2019, 06, 19, 09, 30, 15), color=color.lime)
plot(timestamp("GMT+3", 2019, 06, 19, 09, 30, 15), color=color.fuchsia)
plot(timestamp("Feb 01 2020 22:10:05"))
plot(timestamp("2011-10-10T14:48:00"))
plot(timestamp("04 Dec 1995 00:12:00 GMT+5"))
Returns
UNIX time.
Remarks
UNIX time is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
See also
time()
time
timenow
syminfo.timezone
volume_row.buy_volume()

## [Calculates the total "buy" volume for the volume footprint row represented by a volume_row object.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatesthetotalbuyvolumeforthevolumefootprintrowrepresentedbyavolume_rowobject.)

Calculates the total "buy" volume for the volume footprint row represented by a volume_row object.
Syntax
volume_row.buy_volume(id) → series float
Arguments
id (volume_row) The reference (ID) of the volume_row object to analyze.
Returns
The total "buy" volume for the footprint row.
volume_row.delta()

## [Calculates the volume delta for the volume footprint row represented by a volume_row object. The value represents the difference between the row's "buy" volume and "sell" volume. A positive value indicates that the "buy" volume for the row exceeds the "sell" volume, and a negative value indicates the opposite.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatesthevolumedeltaforthevolumefootprintrowrepresentedbyavolume_rowobject.Thevaluerepresentsthedifferencebetweentherowsbuyvolumeandsellvolume.Apositivevalueindicatesthatthebuyvolumefortherowexceedsthesellvolumeandanegativevalueindicatestheopposite.)

Calculates the volume delta for the volume footprint row represented by a volume_row object. The value represents the difference between the row's "buy" volume and "sell" volume. A positive value indicates that the "buy" volume for the row exceeds the "sell" volume, and a negative value indicates the opposite.
Syntax
volume_row.delta(id) → series float
Arguments
id (volume_row) The reference (ID) of the volume_row object to analyze.
Returns
The volume delta for the footprint row.
volume_row.down_price()

## [Retrieves the lower price level of the volume footprint row represented by a volume_row object.](https://www.tradingview.com/pine-script-reference/v6/#var_Retrievesthelowerpricelevelofthevolumefootprintrowrepresentedbyavolume_rowobject.)

Retrieves the lower price level of the volume footprint row represented by a volume_row object.
Syntax
volume_row.down_price(id) → series float
Arguments
id (volume_row) The reference (ID) of the volume_row object to analyze.
Returns
The lower boundary of the footprint row's price range.
volume_row.has_buy_imbalance()

## [Checks whether the volume footprint row represented by a volume_row object has a "buy" imbalance, based on the imbalance_percent argument of the request.footprint() call that the object depends on. Returns true if the row's "buy" volume exceeds the "sell" volume of the row below it in the footprint by the specified percentage, and false otherwise.](https://www.tradingview.com/pine-script-reference/v6/#var_Checkswhetherthevolumefootprintrowrepresentedbyavolume_rowobjecthasabuyimbalancebasedontheimbalance_percentargumentoftherequest.footprintcallthattheobjectdependson.Returnstrueiftherowsbuyvolumeexceedsthesellvolumeoftherowbelowitinthefootprintbythespecifiedpercentageandfalseotherwise.)

Checks whether the volume footprint row represented by a volume_row object has a "buy" imbalance, based on the imbalance_percent argument of the request.footprint() call that the object depends on. Returns true if the row's "buy" volume exceeds the "sell" volume of the row below it in the footprint by the specified percentage, and false otherwise.
Syntax
volume_row.has_buy_imbalance(id) → series bool
Arguments
id (volume_row) The reference (ID) of the volume_row object to analyze.
Returns
A value of true if the footprint row has a detected buy imbalance, and false otherwise.
volume_row.has_sell_imbalance()

## [Checks whether the volume footprint row represented by a volume_row object has a sell imbalance, based on the imbalance_percent argument of the request.footprint() call that the object depends on. Returns true if the row's "sell" volume exceeds the "buy" volume of the row above it in the footprint by the specified percentage, and false otherwise.](https://www.tradingview.com/pine-script-reference/v6/#var_Checkswhetherthevolumefootprintrowrepresentedbyavolume_rowobjecthasasellimbalancebasedontheimbalance_percentargumentoftherequest.footprintcallthattheobjectdependson.Returnstrueiftherowssellvolumeexceedsthebuyvolumeoftherowaboveitinthefootprintbythespecifiedpercentageandfalseotherwise.)

Checks whether the volume footprint row represented by a volume_row object has a sell imbalance, based on the imbalance_percent argument of the request.footprint() call that the object depends on. Returns true if the row's "sell" volume exceeds the "buy" volume of the row above it in the footprint by the specified percentage, and false otherwise.
Syntax
volume_row.has_sell_imbalance(id) → series bool
Arguments
id (volume_row) The reference (ID) of the volume_row object to analyze.
Returns
A value of true if the footprint row has a detected sell imbalance, and false otherwise.
volume_row.sell_volume()

## [Calculates the total "sell" volume for the volume footprint row represented by a volume_row object.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatesthetotalsellvolumeforthevolumefootprintrowrepresentedbyavolume_rowobject.)

Calculates the total "sell" volume for the volume footprint row represented by a volume_row object.
Syntax
volume_row.sell_volume(id) → series float
Arguments
id (volume_row) The reference (ID) of the volume_row object to analyze.
Returns
The total "sell" volume for the footprint row.
volume_row.total_volume()

## [Calculates the sum of the "buy" and "sell" volume for the volume footprint row represented by a volume_row object.](https://www.tradingview.com/pine-script-reference/v6/#var_Calculatesthesumofthebuyandsellvolumeforthevolumefootprintrowrepresentedbyavolume_rowobject.)

Calculates the sum of the "buy" and "sell" volume for the volume footprint row represented by a volume_row object.
Syntax
volume_row.total_volume(id) → series float
Arguments
id (volume_row) The reference (ID) of the volume_row object to analyze.
Returns
The total volume for the footprint row.
volume_row.up_price()

## [Retrieves the upper price level of the volume footprint row represented by a volume_row object.](https://www.tradingview.com/pine-script-reference/v6/#var_Retrievestheupperpricelevelofthevolumefootprintrowrepresentedbyavolume_rowobject.)

Retrieves the upper price level of the volume footprint row represented by a volume_row object.
Syntax
volume_row.up_price(id) → series float
Arguments
id (volume_row) The reference (ID) of the volume_row object to analyze.
Returns
The upper boundary of the footprint row's price range.
weekofyear()

## [Calculates the week number of the year, in a specified time zone, from a UNIX timestamp.](https://www.tradingview.com/pine-script-reference/v6/#var_CalculatestheweeknumberoftheyearinaspecifiedtimezonefromaUNIXtimestamp.)

Calculates the week number of the year, in a specified time zone, from a UNIX timestamp.
Syntax
weekofyear(time, timezone) → series int
Arguments
time (series int) A UNIX timestamp in milliseconds.
timezone (series string) Optional. Specifies the time zone of the returned week number. The value can be a time zone string in UTC/GMT offset notation (e.g., "UTC-5") or IANA time zone database notation (e.g., "America/New_York"). The default is syminfo.timezone.
Returns
The calculated week number, expressed in the specified time zone.
Remarks
A UNIX timestamp represents the number of milliseconds elapsed since 00:00 UTC on 1970-01-01. The meaning of a UNIX timestamp does not change relative to any time zone.
See also
weekofyear
dayofmonth()
dayofweek()
time()
year()
month()
hour()
minute()
second()
year()

## [Syntax](https://www.tradingview.com/pine-script-reference/v6/#var_Syntax)

Syntax
year(time, timezone) → series int
Arguments
time (series int) UNIX time in milliseconds.
timezone (series string) Allows adjusting the returned value to a time zone specified in either UTC/GMT notation (e.g., "UTC-5", "GMT+0530") or as an IANA time zone database name (e.g., "America/New_York"). Optional. The default is syminfo.timezone.
Returns
Year (in exchange timezone) for provided UNIX time.
Remarks
UNIX time is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
Note that this function returns the year based on the time of the bar's open. For overnight sessions (e.g. EURUSD, where Monday session starts on Sunday, 17:00 UTC-4) this value can be lower by 1 than the year of the trading day.
See also
year
time()
month()
dayofmonth()
dayofweek()
hour()
minute()
second()
