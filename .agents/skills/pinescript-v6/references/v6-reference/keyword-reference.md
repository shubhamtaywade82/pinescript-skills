# Pine Script® v6 Keywords Reference

Source: https://www.tradingview.com/pine-script-reference/v6/

## [and](https://www.tradingview.com/pine-script-reference/v6/#keyword_and)

and

## [Logical AND. Applicable to boolean expressions.](https://www.tradingview.com/pine-script-reference/v6/#keyword_LogicalAND.Applicabletobooleanexpressions.)

Logical AND. Applicable to boolean expressions.
Syntax
expr1 and expr2
Returns
Boolean value, or series of boolean values.
Remarks
If expr1 evaluates to false, the and operator returns false without evaluating expr2.
enum

## [This keyword allows the creation of an enumeration, enum for short. Enums are unique constructs that hold groups of predefined constants.](https://www.tradingview.com/pine-script-reference/v6/#keyword_Thiskeywordallowsthecreationofanenumerationenumforshort.Enumsareuniqueconstructsthatholdgroupsofpredefinedconstants.)

This keyword allows the creation of an enumeration, enum for short. Enums are unique constructs that hold groups of predefined constants.
Each field in an enum has a const string title. Scripts can access the fields in an enum using dot notation, similar to accessing the fields of a user-defined type.
Each field represents a value of the enumName enum. Scripts can declare each field in an enum with an optional const string title. If a field's title is not specified, its title is the string representation of its name. Use str.tostring() on an enum field to retrieve its title.
Syntax
[export ]enum <enumName>
<field_1> [= <title_1>]
<field_2> [= <title_2>]
...
<field_N> [= <title_N>]
One can use an enum to quickly create a dropdown input with the help of the input.enum() function. The options that appear in the dropdown represent the titles of the enum fields.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("Session highlight", overlay = true)

## [//@enum       Contains fields with popular timezones as titles.](https://www.tradingview.com/pine-script-reference/v6/#keyword_enumContainsfieldswithpopulartimezonesastitles.)

//@enum       Contains fields with popular timezones as titles.
//@field exch Has an empty string as the title to represent the chart timezone.
enum tz
    utc  = "UTC"
    exch = ""
    ny   = "America/New_York"
    chi  = "America/Chicago"
    lon  = "Europe/London"
    tok  = "Asia/Tokyo"

## [//@variable The session string.](https://www.tradingview.com/pine-script-reference/v6/#keyword_variableThesessionstring.)

//@variable The session string.
selectedSession = input.session("1200-1500", "Session")
//@variable The selected timezone. The input's dropdown contains the fields in the `tz` enum.
selectedTimezone = input.enum(tz.utc, "Session Timezone")

## [//@variable Is `true` if the current bar's time is in the specified session.](https://www.tradingview.com/pine-script-reference/v6/#keyword_variableIstrueifthecurrentbarstimeisinthespecifiedsession.)

//@variable Is `true` if the current bar's time is in the specified session.
bool inSession = false
if not na(time("", selectedSession, str.tostring(selectedTimezone)))
    inSession := true

## [// Highlight the background when `inSession` is `true`.](https://www.tradingview.com/pine-script-reference/v6/#keyword_HighlightthebackgroundwheninSessionistrue.)

// Highlight the background when `inSession` is `true`.
bgcolor(inSession ? color.new(color.green, 90) : na, title = "Active session highlight")
Additionally, one can use an enum in a collection's type template to restrict the values it will allow as elements. When used inside a type template, the collection will only accept fields that belong to the specified enum.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("Map with enum keys")

## [//@enum        Contains fields with titles representing ticker IDs.](https://www.tradingview.com/pine-script-reference/v6/#keyword_enumContainsfieldswithtitlesrepresentingtickerIDs.)

//@enum        Contains fields with titles representing ticker IDs.
//@field aapl  Has an Apple ticker ID as its title.
//@field tsla  Has a Tesla ticker ID as its title.
//@field amzn  Has an Amazon ticker ID as its title.
enum symbols
    aapl = "NASDAQ:AAPL"
    tsla = "NASDAQ:TSLA"
    amzn = "NASDAQ:AMZN"

## [//@variable A map that accepts fields from the `symbols` enum as keys and "float" values.](https://www.tradingview.com/pine-script-reference/v6/#keyword_variableAmapthatacceptsfieldsfromthesymbolsenumaskeysandfloatvalues.)

//@variable A map that accepts fields from the `symbols` enum as keys and "float" values.
map<symbols, float> data = map.new<symbols, float>()
// Put key-value pairs into the `data` map.
data.put(symbols.aapl, request.security(str.tostring(symbols.aapl), timeframe.period, close))
data.put(symbols.tsla, request.security(str.tostring(symbols.tsla), timeframe.period, close))
data.put(symbols.amzn, request.security(str.tostring(symbols.amzn), timeframe.period, close))
// Plot the value from the `data` map accessed by the `symbols.aapl` key.
plot(data.get(symbols.aapl))
export

## [Used in libraries to prefix the declaration of functions or user-defined type definitions that will be available from other scripts importing the library.](https://www.tradingview.com/pine-script-reference/v6/#keyword_Usedinlibrariestoprefixthedeclarationoffunctionsoruser-definedtypedefinitionsthatwillbeavailablefromotherscriptsimportingthelibrary.)

Used in libraries to prefix the declaration of functions or user-defined type definitions that will be available from other scripts importing the library.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
//@description Library of debugging functions.
library("Debugging_library", overlay = true)
//@function Displays a string as a table cell for debugging purposes.
//@param txt String to display.
//@returns Void.
export print(string txt) =>
    var table t = table.new(position.middle_right, 1, 1)
    table.cell(t, 0, 0, txt, bgcolor = color.yellow)
// Using the function from inside the library to show an example on the published chart.
// This has no impact on scripts using the library.
print("Library Test")
Remarks
Each library must have at least one exported function or user-defined type (UDT).
Exported functions cannot use variables from the global scope if they are arrays, mutable variables (reassigned with :=), or variables of 'input' form.
Exported functions cannot use request.*() functions.
Exported functions must explicitly declare each parameter's type and all parameters must be used in the function's body. By default, all arguments passed to exported functions are of the series form, unless they are explicitly specified as simple in the function's signature.
The @description, @function, @param, @type, @field, and @returns compiler annotations are used to automatically generate the library's description and release notes, and in the Pine Script® Editor's tooltips.
See also
library()
import
simple
series
type
for

## [Creates a count-controlled loop, which uses a counter variable to manage the iterative executions of its local code block. The loop continues new iterations until the counter reaches a specified final value.](https://www.tradingview.com/pine-script-reference/v6/#keyword_Createsacount-controlledloopwhichusesacountervariabletomanagetheiterativeexecutionsofitslocalcodeblock.Theloopcontinuesnewiterationsuntilthecounterreachesaspecifiedfinalvalue.)

Creates a count-controlled loop, which uses a counter variable to manage the iterative executions of its local code block. The loop continues new iterations until the counter reaches a specified final value.
Syntax
[variables =|:=] for counter = from_num to to_num [by step_num]
    statements | continue | break
    return_expression
variables (return_expression type) Optional. A variable or tuple to hold the values or references from the last evaluation of return_expression after the loop terminates. The script can assign the loop's returned results to variables only if the results are not of the "void" type. If the loop's conditions prevent iteration, or if no iterations evaluate return_expression, the variables' assigned values or references are na, or false if the return type is "bool".
counter (series int/float) The counter variable. The loop increments the variable's value from the initial value (from_num) to the final value (to_num) by a fixed amount (step_num) after each iteration. The last possible iteration occurs when the variable's value reaches the to_num value.
from_num (series int/float) The value of the counter variable on the loop's first iteration.
to_num (series int/float) The final counter value for which the loop's header allows a new iteration. The loop increments the counter value by the step_num until it reaches or passes this value. If the script modifies this value during a loop iteration, the loop header uses the new value to control the allowed subsequent iterations.
step_num (series int/float) Optional. A positive value specifying the amount by which the counter value increases or decreases until it reaches or passes the to_num value. If the from_num value is greater than the initial to_num value, the loop subtracts this amount from the counter value after each iteration. Otherwise, the loop adds this amount after each iteration. The default is 1.
statements The code statements and expressions within the loop's body, i.e., the indented block of code beneath the loop header.
return_expression (any type) The last expression or statement within the loop's body. The loop returns the results from this code after the final iteration. If the loop stops prematurely due to a continue or break statement, the returned values or references are those of the latest iteration that evaluated this code. To use the loop's returned results, assign them to a variable or tuple.
continue A loop-specific keyword that instructs the script to skip the remainder of the current loop iteration and continue to the next iteration.
break A loop-specific keyword that prompts the script to stop the current iteration and exit the loop entirely.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("Basic `for` loop")

## [//@function Calculates the number of bars in the last `length` bars that have their `close` above the current `close`.](https://www.tradingview.com/pine-script-reference/v6/#keyword_functionCalculatesthenumberofbarsinthelastlengthbarsthathavetheircloseabovethecurrentclose.)

//@function Calculates the number of bars in the last `length` bars that have their `close` above the current `close`.
//@param length The number of bars used in the calculation.
greaterCloseCount(length) =>
    int result = 0
    for i = 1 to length
        if close[i] > close
            result += 1
    result

## [plot(greaterCloseCount(14))](https://www.tradingview.com/pine-script-reference/v6/#keyword_plotgreaterCloseCount14)

plot(greaterCloseCount(14))
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("`for` loop with a step")

## [a = array.from(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)](https://www.tradingview.com/pine-script-reference/v6/#keyword_aarray.from0123456789)

a = array.from(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
sum = 0.0

## [for i = 0 to 9 by 5](https://www.tradingview.com/pine-script-reference/v6/#keyword_fori0to9by5)

for i = 0 to 9 by 5
    // Because the step is set to 5, we are adding only the first (0) and the sixth (5) value from the array `a`.
    sum += array.get(a, i)

## [plot(sum)](https://www.tradingview.com/pine-script-reference/v6/#keyword_plotsum)

plot(sum)
Remarks
Modifying a loop's to_num value during an iteration does not change the direction of the loop's counter. For a loop that counts upward, setting the to_num to a value less than the from_num value on an iteration stops the loop immediately after that iteration ends. Likewise, a loop that counts downward stops after an iteration where the to_num value becomes greater than the from_num value.
If a script initializes a variable declared with var or varip using a loop's result, the loop stops after the first iteration, even if the header's criteria allow more iterations. Instead of initializing the variable with the result of this structure directly, declare the variable first and use := to update it with the result. Alternatively, move the loop into a Declaring functions and initialize the variable using a call to that function.
See the Loops page of our User Manual to learn more about loops and how they work.
See also
for...in
while
for...in

## [Creates a collection-controlled loop, which iterates over the elements of an array, the rows of a matrix, or the key-value pairs of a map in order. The loop's local code block executes once for each element, row, or pair in the specified collection.](https://www.tradingview.com/pine-script-reference/v6/#keyword_Createsacollection-controlledloopwhichiteratesovertheelementsofanarraytherowsofamatrixorthekey-valuepairsofamapinorder.Theloopslocalcodeblockexecutesonceforeachelementroworpairinthespecifiedcollection.)

Creates a collection-controlled loop, which iterates over the elements of an array, the rows of a matrix, or the key-value pairs of a map in order. The loop's local code block executes once for each element, row, or pair in the specified collection.
Syntax
[variables = | :=] for item in collection_id
    statements | continue | break
    return_expression

## [[variables = | :=] for [index, item] in collection_id](https://www.tradingview.com/pine-script-reference/v6/#keyword_variablesforindexitemincollection_id)

[variables = | :=] for [index, item] in collection_id
    statements | continue | break
    return_expression
variables (return_expression type) - Optional. A variable or tuple to hold the values or references from the last evaluation of return_expression after the loop terminates. The script can assign the loop's returned results to variables only if the results are not of the "void" type. If the loop's conditions prevent iteration, or if no iterations evaluate return_expression, the variables' assigned values or references are na, or false if the return type is "bool".
index - A variable to track the array element index, matrix row index, or map key of the current iteration. The loop cannot modify this variable using reassignment or compound assignment operators. This variable is valid only in the second form of the loop structure.
item - A variable to track the array element, matrix row, or map value element of the current iteration. The loop cannot modify this variable using reassignment or compound assignment operators.
collection_id (array/matrix/map) - The ID of the array, matrix, or map whose items the loop iterates over.
statements - The code statements and expressions within the loop's body, i.e., the indented block of code beneath the loop header.
return_expression (any type) - The last expression or statement within the loop's body. The loop returns the results from this code after the final iteration. If the loop stops prematurely due to a continue or break statement, the returned values or references are those of the latest iteration that evaluated this code. To use the loop's returned results, assign them to a variable or tuple.
continue - A loop-specific keyword that instructs the script to skip the remainder of the current loop iteration and continue to the next iteration.
break - A loop-specific keyword that prompts the script to stop the current iteration and exit the loop entirely.
The following example uses the first form of the for...in loop to count the number of array element values that are greater than a specified value:
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("'for...in' array (first form) demo")

## [//@function Counts the number of 'id' array elements that are greater than the specified value.](https://www.tradingview.com/pine-script-reference/v6/#keyword_functionCountsthenumberofidarrayelementsthataregreaterthanthespecifiedvalue.)

//@function Counts the number of 'id' array elements that are greater than the specified value.
numGreaterThan(array<float> id, float value) =>
    int result = 0
    for element in id
        if element > value
            result += 1
    result

## [//@variable References an array containing the current bar's OHLC values.](https://www.tradingview.com/pine-script-reference/v6/#keyword_variableReferencesanarraycontainingthecurrentbarsOHLCvalues.)

//@variable References an array containing the current bar's OHLC values.
array<float> ohlcValues = array.from(open, high, low, close)

## [// Plot the number of 'ohlcValues' elements that are greater than the 20-bar SMA of 'close'.](https://www.tradingview.com/pine-script-reference/v6/#keyword_PlotthenumberofohlcValueselementsthataregreaterthanthe20-barSMAofclose.)

// Plot the number of 'ohlcValues' elements that are greater than the 20-bar SMA of 'close'.
plot(numGreaterThan(ohlcValues, ta.sma(close, 20)))
The example below uses the second form of the loop structure to perform element-wise addition between two arrays:
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("`for...in` array (second form) demo")

## [//@function Creates a new array whose elements are the sums of corresponding elements in the `id1` and `id2` arrays.](https://www.tradingview.com/pine-script-reference/v6/#keyword_functionCreatesanewarraywhoseelementsarethesumsofcorrespondingelementsintheid1andid2arrays.)

//@function Creates a new array whose elements are the sums of corresponding elements in the `id1` and `id2` arrays.
elementWiseAdd(array<float> id1, array<float> id2) =>
    array<float> result = array.new<float>()
    // Loop through the `id1` array while tracking each element's index *and* value.
    for [index, element1] in id1
        // Use `index` to retrieve the corresponding element in the `id2` array, then push the sum into the new array.
        float element2 = id2.get(index)
        result.push(element1 + element2)
    result

## [if barstate.isfirst](https://www.tradingview.com/pine-script-reference/v6/#keyword_ifbarstate.isfirst)

if barstate.isfirst
    // Create two arrays for which to perform element-wise addition.
    array<float> array1 = array.from(1.0, 2.0, 3.0, 4.0)
    array<float> array2 = array.from(2.0, 3.0, 4.0, 5.0)

## [//@variable References the resulting array of element-wise sums.](https://www.tradingview.com/pine-script-reference/v6/#keyword_variableReferencestheresultingarrayofelement-wisesums.)

//@variable References the resulting array of element-wise sums.
    array<float> sums = elementWiseAdd(array1, array2)
    // Log a string representation of the `sums` array's contents in the Pine Logs pane.
    log.info(str.tostring(sums))
This example uses the first form of the loop structure to iterate over the rows of a matrix and create an array containing each one's sum. The header's loop variable, rowArrayID, references an array containing the current row's values:
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("`for...in` matrix (first form) demo")

## [//@function Creates a matrix that organizes the contents of the `arrayID` array into a specified shape.](https://www.tradingview.com/pine-script-reference/v6/#keyword_functionCreatesamatrixthatorganizesthecontentsofthearrayIDarrayintoaspecifiedshape.)

//@function Creates a matrix that organizes the contents of the `arrayID` array into a specified shape.
matrixFromArray(array<float> arrayID, int rows, int columns) =>
    matrix<float> result = matrix.new<float>()
    result.add_row(0, arrayID)
    result.reshape(rows, columns)
    result

## [//@function Creates an array containing the sum of elements in each row of the `matrixID` matrix.](https://www.tradingview.com/pine-script-reference/v6/#keyword_functionCreatesanarraycontainingthesumofelementsineachrowofthematrixIDmatrix.)

//@function Creates an array containing the sum of elements in each row of the `matrixID` matrix.
calcRowSums(matrix<float> matrixID) =>
    array<float> result = array.new<float>()
    // Iterate over the matrix rows, where `rowArrayID` references an *array* containing the current row's values.
    for rowArrayID in matrixID
        // Push the sum of `rowArrayID` elements into the `result` array.
        result.push(rowArrayID.sum())
    result

## [if barstate.isfirst](https://www.tradingview.com/pine-script-reference/v6/#keyword_ifbarstate.isfirst)

if barstate.isfirst
    // Create a 2x2 matrix of pseudorandom values.
    array<float>  randArray = array.from(math.random(), math.random(), math.random(), math.random())
    matrix<float> randMat   = matrixFromArray(randArray, 2, 2)
    // Log string representation of the `randMat` matrix and the calculated array of row sums in the Pine Logs pane.
    log.info("\n" + str.tostring(randMat))
    log.info(str.tostring(calcRowSums(randMat)))
The following example uses a for...in loop to iterate over a map's key-value pairs and construct a custom string representation of its contents:
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("`for...in` map demo")

## [//@function Creates a custom string representation of a map containing "string" keys and "float" values.](https://www.tradingview.com/pine-script-reference/v6/#keyword_functionCreatesacustomstringrepresentationofamapcontainingstringkeysandfloatvalues.)

//@function Creates a custom string representation of a map containing "string" keys and "float" values.
toString(map<string, float> id) =>
    string result = "{"
    // Iterate through the key-value pairs of the `id` map, in insertion order.
    for [key, value] in id
        result += str.format("''{0}'': {1}, ", key, value)
    result += "}"
    result := str.replace(result, ", }", "}")

## [if barstate.islastconfirmedhistory](https://www.tradingview.com/pine-script-reference/v6/#keyword_ifbarstate.islastconfirmedhistory)

if barstate.islastconfirmedhistory
    //@variable References a map to store "float" OHLC values with corresponding "string" keys.
    map<string, float> ohlcMap = map.new<string, float>()
    // Put key-value pairs into the map.
    ohlcMap.put("Open",  open)
    ohlcMap.put("High",  high)
    ohlcMap.put("Low",   low)
    ohlcMap.put("Close", close)
    // Log the `toString()` result for the map referenced by `ohlcMap`.
    log.info(toString(ohlcMap))
Remarks
Only the second form of the for...in loop is compatible with maps. The loop iterates over the key-value pairs of a map in the insertion order of its keys.
Scripts can modify the sizes of arrays and matrices while iterating over their contents with a for...in loop. However, they cannot modify the sizes of maps while looping over them directly with this structure. To modify a map while using a for...in loop, use the loop on a copy of the map or on the map's map.keys() array.
If a script initializes a variable declared with var or varip using a loop's result, the loop stops after the first iteration, even if the header's criteria allow more iterations. Instead of initializing the variable with the result of this structure directly, declare the variable first and use := to update it with the result. Alternatively, move the loop into a Declaring functions and initialize the variable using a call to that function.
See the Loops page of our User Manual to learn more about loops and how they work.
See also
for
while
array.sum()
array.min()
array.max()
if

## [If statement defines what block of statements must be executed when conditions of the expression are satisfied.](https://www.tradingview.com/pine-script-reference/v6/#keyword_Ifstatementdefineswhatblockofstatementsmustbeexecutedwhenconditionsoftheexpressionaresatisfied.)

If statement defines what block of statements must be executed when conditions of the expression are satisfied.
To have access to and use the if statement, one should specify the version >= 2 of Pine Script® language in the very first line of code, for example: //@version=6
The 4th version of Pine Script® Language allows you to use “else if” syntax.
General code form:
Syntax
var_declarationX = if condition
    var_decl_then0
    var_decl_then1
    …
    var_decl_thenN
else if [optional block]
    var_decl_else0
    var_decl_else1
    …
    var_decl_elseN
else
    var_decl_else0
    var_decl_else1
    …
    var_decl_elseN
    return_expression_else
where
var_declarationX — this variable gets the value of the if statement
condition — if the condition is true, the logic from the block 'then' (var_decl_then0, var_decl_then1, etc.) is used.
If the condition is false, the logic from the block 'else' (var_decl_else0, var_decl_else1, etc.) is used.
return_expression_then, return_expression_else — the last expression from the block then or from the block else will return the final value of the statement. If declaration of the variable is in the end, its value will be the result.
The type of returning value of the if statement depends on return_expression_then and return_expression_else type (their types must match: it is not possible to return an integer value from then, while you have a string value in else block).
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("if")
// This code compiles
x = if close > open
    close
else
    open

## [// This code doesn’t compile](https://www.tradingview.com/pine-script-reference/v6/#keyword_Thiscodedoesntcompile)

// This code doesn’t compile
// y = if close > open
//     close
// else
//     "open"
plot(x)
It is possible to omit the else block. In this case if the condition is false, an “empty” value (na, false, or “”) will be assigned to the var_declarationX variable:
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("if")
x = if close > open
    close
// If current close > current open, then x = close.
// Otherwise the x = na.
plot(x)
It is possible to use either multiple “else if” blocks or none at all. The blocks “then”, “else if”, “else” are shifted by four spaces:
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("if")
x = if open > close
    5
else if high > low
    close
else
    open
plot(x)
It is possible to ignore the resulting value of an if statement (“var_declarationX=“ can be omitted). It may be useful if you need the side effect of the expression, for example in strategy trading:
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
strategy("if")
if (ta.crossover(high, low))
    strategy.entry("BBandLE", strategy.long, stop=low, oca_name="BollingerBands", oca_type=strategy.oca.cancel, comment="BBandLE")
else
    strategy.cancel(id="BBandLE")
If statements can include each other:
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("if")
float x = na
if close > open
    if close > close[1]
        x := close
    else
        x := close[1]
else
    x := open
plot(x)
import

## [Used to load an external library() into a script and bind its functions to a namespace. The importing script can be an indicator, a strategy, or another library. A library must be published (privately or publicly) before it can be imported.](https://www.tradingview.com/pine-script-reference/v6/#keyword_Usedtoloadanexternallibraryintoascriptandbinditsfunctionstoanamespace.Theimportingscriptcanbeanindicatorastrategyoranotherlibrary.Alibrarymustbepublishedprivatelyorpubliclybeforeitcanbeimported.)

Used to load an external library() into a script and bind its functions to a namespace. The importing script can be an indicator, a strategy, or another library. A library must be published (privately or publicly) before it can be imported.
Syntax
import {username}/{libraryName}/{libraryVersion} as {alias}
Arguments
username (literal string) User name of the library's author.
libraryName (literal string) Name of the imported library, which corresponds to the title argument used by the author in his library script.
libraryVersion (literal int) Version number of the imported library.
alias (literal string) A non-numeric identifier used as a namespace to refer to the library's functions. Optional. The default is the libraryName string.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("num_methods import")
// Import the first version of the username’s "num_methods" library and assign it to the "m" namespace",
import username/num_methods/1 as m
// Call the “sinh()” function from the imported library
y = m.sinh(3.14)
// Plot value returned by the "sinh()" function",
plot(y)
Remarks
Using an alias that replaces a built-in namespace such as math.*or strategy.* is allowed, but if the library contains function names that shadow Pine Script®'s built-in functions, the built-ins will become unavailable. The same version of a library can only be imported once. Aliases must be distinct for each imported library. When calling library functions, casting their arguments to types other than their declared type is not allowed. An import statement cannot use 'as' or 'import' as username, libraryName, or alias identifiers.
See also
library()
export
method

## [This keyword is used to prefix a function declaration, indicating it can then be invoked using dot notation by appending its name to a variable of the type of its first parameter and omitting that first parameter. Alternatively, functions declared as methods can also be invoked like normal user-defined functions. In that case, an argument must be supplied for its first parameter.](https://www.tradingview.com/pine-script-reference/v6/#keyword_Thiskeywordisusedtoprefixafunctiondeclarationindicatingitcanthenbeinvokedusingdotnotationbyappendingitsnametoavariableofthetypeofitsfirstparameterandomittingthatfirstparameter.Alternativelyfunctionsdeclaredasmethodscanalsobeinvokedlikenormaluser-definedfunctions.Inthatcaseanargumentmustbesuppliedforitsfirstparameter.)

This keyword is used to prefix a function declaration, indicating it can then be invoked using dot notation by appending its name to a variable of the type of its first parameter and omitting that first parameter. Alternatively, functions declared as methods can also be invoked like normal user-defined functions. In that case, an argument must be supplied for its first parameter.
The first parameter of a method declaration must be explicitly typified.
Syntax
[export] method <functionName>(<paramType> <paramName> [= <defaultValue>], …) =>
    <functionBlock>
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("")

## [var prices = array.new<float>()](https://www.tradingview.com/pine-script-reference/v6/#keyword_varpricesarray.newfloat)

var prices = array.new<float>()

## [//@function Pushes a new value into the array and removes the first one if the resulting array is greater than `maxSize`. Can be used as a method.](https://www.tradingview.com/pine-script-reference/v6/#keyword_functionPushesanewvalueintothearrayandremovesthefirstoneiftheresultingarrayisgreaterthanmaxSize.Canbeusedasamethod.)

//@function Pushes a new value into the array and removes the first one if the resulting array is greater than `maxSize`. Can be used as a method.
method maintainArray(array<float> id, maxSize, value) =>
    id.push(value)
    if id.size() > maxSize
        id.shift()

## [prices.maintainArray(50, close)](https://www.tradingview.com/pine-script-reference/v6/#keyword_prices.maintainArray50close)

prices.maintainArray(50, close)
// The method can also be called like a function, without using dot notation.
// In this case an argument must be supplied for its first parameter.
// maintainArray(prices, 50, close)

## [// This calls the `array.avg()` built-in using dot notation with the `prices` array.](https://www.tradingview.com/pine-script-reference/v6/#keyword_Thiscallsthearray.avgbuilt-inusingdotnotationwiththepricesarray.)

// This calls the `array.avg()` built-in using dot notation with the `prices` array.
// It is possible because built-in functions belonging to some namespaces that are a special Pine type
// can be invoked with method notation when the function's first parameter is an ID of that type.
// Those namespaces are: `array`, `matrix`, `line`, `linefill`, `label`, `box`, and `table`.
plot(prices.avg())
not

## [Logical negation (NOT). Applicable to boolean expressions.](https://www.tradingview.com/pine-script-reference/v6/#keyword_LogicalnegationNOT.Applicabletobooleanexpressions.)

Logical negation (NOT). Applicable to boolean expressions.
Syntax
not expr1
Returns
Boolean value, or series of boolean values.
or

## [Logical OR. Applicable to boolean expressions.](https://www.tradingview.com/pine-script-reference/v6/#keyword_LogicalOR.Applicabletobooleanexpressions.)

Logical OR. Applicable to boolean expressions.
Syntax
expr1 or expr2
Returns
Boolean value, or series of boolean values.
Remarks
If expr1 evaluates to true, the or operator returns true without evaluating expr2.
switch

## [The switch operator transfers control to one of the several statements, depending on the values of a condition and expressions.](https://www.tradingview.com/pine-script-reference/v6/#keyword_Theswitchoperatortransferscontroltooneoftheseveralstatementsdependingonthevaluesofaconditionandexpressions.)

The switch operator transfers control to one of the several statements, depending on the values of a condition and expressions.
Syntax
[variable_declaration = ] switch expression
    value1 => local_block
    value2 => local_block
    …
    => default_local_block

## [[variable_declaration = ] switch](https://www.tradingview.com/pine-script-reference/v6/#keyword_variable_declarationswitch)

[variable_declaration = ] switch
    condition1 => local_block
    condition2 => local_block
    …
    => default_local_block
Switch with an expression:
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("Switch using an expression")

## [string i_maType = input.string("EMA", "MA type", options = ["EMA", "SMA", "RMA", "WMA"])](https://www.tradingview.com/pine-script-reference/v6/#keyword_stringi_maTypeinput.stringEMAMAtypeoptionsEMASMARMAWMA)

string i_maType = input.string("EMA", "MA type", options = ["EMA", "SMA", "RMA", "WMA"])

## [float ma = switch i_maType](https://www.tradingview.com/pine-script-reference/v6/#keyword_floatmaswitchi_maType)

float ma = switch i_maType
    "EMA" => ta.ema(close, 10)
    "SMA" => ta.sma(close, 10)
    "RMA" => ta.rma(close, 10)
    // Default used when the three first cases do not match.
    => ta.wma(close, 10)

## [plot(ma)](https://www.tradingview.com/pine-script-reference/v6/#keyword_plotma)

plot(ma)
Switch without an expression:
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
strategy("Switch without an expression", overlay = true)

## [bool longCondition  = ta.crossover( ta.sma(close, 14), ta.sma(close, 28))](https://www.tradingview.com/pine-script-reference/v6/#keyword_boollongConditionta.crossoverta.smaclose14ta.smaclose28)

bool longCondition  = ta.crossover( ta.sma(close, 14), ta.sma(close, 28))
bool shortCondition = ta.crossunder(ta.sma(close, 14), ta.sma(close, 28))

## [switch](https://www.tradingview.com/pine-script-reference/v6/#keyword_switch)

switch
    longCondition  => strategy.entry("Long ID", strategy.long)
    shortCondition => strategy.entry("Short ID", strategy.short)
Returns
The value of the last expression in the local block of statements that is executed.
Remarks
Only one of the local_block instances or the default_local_block can be executed. The default_local_block is introduced with the => token alone and is only executed when none of the preceding blocks are executed. If the result of the switch statement is assigned to a variable and a default_local_block is not specified, the statement returns na if no local_block is executed. When assigning the result of the switch statement to a variable, all local_block instances must return the same type of value.
See also
if
?:
type

## [This keyword allows the declaration of user-defined types (UDT) from which scripts can instantiate objects. UDTs are composite types that contain an arbitrary number of fields of any built-in or user-defined type, including the defined UDT itself. The syntax to define a UDT is:](https://www.tradingview.com/pine-script-reference/v6/#keyword_Thiskeywordallowsthedeclarationofuser-definedtypesUDTfromwhichscriptscaninstantiateobjects.UDTsarecompositetypesthatcontainanarbitrarynumberoffieldsofanybuilt-inoruser-definedtypeincludingthedefinedUDTitself.ThesyntaxtodefineaUDTis)

This keyword allows the declaration of user-defined types (UDT) from which scripts can instantiate objects. UDTs are composite types that contain an arbitrary number of fields of any built-in or user-defined type, including the defined UDT itself. The syntax to define a UDT is:
Syntax
[export ]type <UDT_identifier>
    [varip ]<field_type> <field_name> [= <value>]
    …
Once a UDT is defined, scripts can instantiate objects from it with the UDT_identifier.new() construct. When creating a new type instance, the fields of the resulting object will initialize with the default values from the UDT's definition. Any type fields without specified defaults will initialize as na. Alternatively, users can pass initial values as arguments in the *.new() method to override the type's defaults. For example, newFooObject = foo.new(x = true) assigns a new foo object to the newFooObject variable with its x field initialized using a value of true.
Field declarations can include the varip keyword, in which case the field values persist between successive script iterations on the same bar.
For more information see the User Manual's sections on defining UDTs and using objects.
Libraries can export UDTs. See the Libraries page of our User Manual to learn more.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("Multi Time Period Chart", overlay = true)

## [timeframeInput = input.timeframe("1D")](https://www.tradingview.com/pine-script-reference/v6/#keyword_timeframeInputinput.timeframe1D)

timeframeInput = input.timeframe("1D")

## [type bar](https://www.tradingview.com/pine-script-reference/v6/#keyword_typebar)

type bar
    float o = open
    float h = high
    float l = low
    float c = close
    int   t = time

## [drawBox(bar b, right) =>](https://www.tradingview.com/pine-script-reference/v6/#keyword_drawBoxbarbright)

drawBox(bar b, right) =>
    color boxColor = b.c >= b.o ? color.green : color.red
    box.new(b.t, b.h, right, b.l, boxColor, xloc = xloc.bar_time, bgcolor = color.new(boxColor, 90))

## [updateBox(box boxId, bar b) =>](https://www.tradingview.com/pine-script-reference/v6/#keyword_updateBoxboxboxIdbarb)

updateBox(box boxId, bar b) =>
    color boxColor = b.c >= b.o ? color.green : color.red
    box.set_border_color(boxId, boxColor)
    box.set_bgcolor(boxId, color.new(boxColor, 90))
    box.set_top(boxId, b.h)
    box.set_bottom(boxId, b.l)
    box.set_right(boxId, time)

## [secBar = request.security(syminfo.tickerid, timeframeInput, bar.new())](https://www.tradingview.com/pine-script-reference/v6/#keyword_secBarrequest.securitysyminfo.tickeridtimeframeInputbar.new)

secBar = request.security(syminfo.tickerid, timeframeInput, bar.new())

## [if not na(secBar)](https://www.tradingview.com/pine-script-reference/v6/#keyword_ifnotnasecBar)

if not na(secBar)
    // To avoid a runtime error, only process data when an object exists.
    if not barstate.islast
        if timeframe.change(timeframeInput)
            // On historical bars, draw a new box in the past when the HTF closes.
            drawBox(secBar, time[1])
    else
        var box lastBox = na
        if na(lastBox) or timeframe.change(timeframeInput)
            // On the last bar, only draw a new current box the first time we get there or when HTF changes.
            lastBox := drawBox(secBar, time)
        else
            // On other chart updates, use setters to modify the current box.
            updateBox(lastBox, secBar)
var

## [var is the keyword used for assigning and one-time initializing of the variable.](https://www.tradingview.com/pine-script-reference/v6/#keyword_varisthekeywordusedforassigningandone-timeinitializingofthevariable.)

var is the keyword used for assigning and one-time initializing of the variable.
Normally, a syntax of assignment of variables, which doesn’t include the keyword var, results in the value of the variable being overwritten with every update of the data. Contrary to that, when assigning variables with the keyword var, they can “keep the state” despite the data updating, only changing it when conditions within if-expressions are met.
Syntax
var variable_name = expression
where:
variable_name - any name of the user’s variable that’s allowed in Pine Script® (can contain capital and lowercase Latin characters, numbers, and underscores (_), but can’t start with a number).
expression - any arithmetic expression, just as with defining a regular variable. The expression will be calculated and assigned to a variable once.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("Var keyword example")
var a = close
var b = 0.0
var c = 0.0
var green_bars_count = 0
if close > open
    var x = close
    b := x
    green_bars_count := green_bars_count + 1
    if green_bars_count >= 10
        var y = close
        c := y
plot(a)
plot(b)
plot(c)
The variable 'a' keeps the closing price of the first bar for each bar in the series.
The variable 'b' keeps the closing price of the first "green" bar in the series.
The variable 'c' keeps the closing price of the tenth "green" bar in the series.
varip

## [varip (var intrabar persist) is the keyword used for the assignment and one-time initialization of a variable or a field of a user-defined type. It’s similar to the var keyword, but variables and fields declared with varip retain their values between executions of the script on the same bar.](https://www.tradingview.com/pine-script-reference/v6/#keyword_varipvarintrabarpersististhekeywordusedfortheassignmentandone-timeinitializationofavariableorafieldofauser-definedtype.Itssimilartothevarkeywordbutvariablesandfieldsdeclaredwithvaripretaintheirvaluesbetweenexecutionsofthescriptonthesamebar.)

varip (var intrabar persist) is the keyword used for the assignment and one-time initialization of a variable or a field of a user-defined type. It’s similar to the var keyword, but variables and fields declared with varip retain their values between executions of the script on the same bar.
Syntax
varip [<variable_type> ]<variable_name> = <expression>

## [[export ]type <UDT_identifier>](https://www.tradingview.com/pine-script-reference/v6/#keyword_exporttypeUDT_identifier)

[export ]type <UDT_identifier>
    varip <field_type> <field_name> [= <value>]
where:
variable_type - An optional fundamental type (int, float, bool, color, string) or a user-defined type, or an array or matrix of one of those types. Special types are not compatible with this keyword.
variable_name - A valid identifier. The variable can also be an object created from a UDT.
expression - Any arithmetic expression, just as when defining a regular variable. The expression will be calculated and assigned to the variable only once, on the first bar.
UDT_identifier, field_type, field_name, value - Constructs related to user-defined types as described in the type section.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("varip")
varip int v = -1
v := v + 1
plot(v)
With var, v would equal the value of the bar_index. On historical bars, where the script calculates only once per chart bar, the value of v is the same as with var. However, on realtime bars, the script will evaluate the expression on each new chart update, producing a different result.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("varip with types")
type barData
    int index = -1
    varip int ticks = -1

## [var currBar = barData.new()](https://www.tradingview.com/pine-script-reference/v6/#keyword_varcurrBarbarData.new)

var currBar = barData.new()
currBar.index += 1
currBar.ticks += 1

## [// Will be equal to bar_index on all bars](https://www.tradingview.com/pine-script-reference/v6/#keyword_Willbeequaltobar_indexonallbars)

// Will be equal to bar_index on all bars
plot(currBar.index)
// In real time, will increment per every tick on the chart
plot(currBar.ticks)
The same += operation applied to both the index and ticks fields results in different real-time values because ticks increases on every chart update, while index only does so once per bar. Note how the currBar object does not use the varip keyword. The ticks field of the object can increment on every tick, but the reference itself is defined once and then stays unchanged. If we were to declare currBar using varip, the behavior of index would remain unchanged because while the reference to the type instance would persist between chart updates, the index field of the object would not.
Remarks
When using varip to declare variables in strategies that may execute more than once per historical chart bar, the values of such variables are preserved across successive iterations of the script on the same bar.
The effect of varip eliminates the rollback of variables before each successive execution of a script on the same bar.
while

## [Creates a condition-controlled loop whose local code block executes repeatedly while the value of a conditional expression remains true. The loop's iterations end after the condition's value becomes false.](https://www.tradingview.com/pine-script-reference/v6/#keyword_Createsacondition-controlledloopwhoselocalcodeblockexecutesrepeatedlywhilethevalueofaconditionalexpressionremainstrue.Theloopsiterationsendaftertheconditionsvaluebecomesfalse.)

Creates a condition-controlled loop whose local code block executes repeatedly while the value of a conditional expression remains true. The loop's iterations end after the condition's value becomes false.
Syntax
[variables = | :=] while condition
    statements | continue | break
    return_expression
variables (return_expression type) - Optional. A variable or tuple to hold the values or references from the last evaluation of return_expression after the loop terminates. The script can assign the loop's returned results to variables only if the results are not of the "void" type. If the loop's conditions prevent iteration, or if no iterations evaluate return_expression, the variables' assigned values or references are na, or false if the return type is "bool".
condition (series bool) - The conditional expression that controls the loop's iterations. If true, the script performs a new iteration. If false, the script exits the loop without performing a new iteration.
statements - The code statements and expressions within the loop's body, i.e., the indented block of code beneath the loop header.
return_expression (any type) - The last expression or statement within the loop's body. The loop returns the results from this code after the final iteration. If the loop stops prematurely due to a continue or break statement, the returned values or references are those of the latest iteration that evaluated this code. To use the loop's returned results, assign them to a variable or tuple.
continue - A loop-specific keyword that instructs the script to skip the remainder of the current loop iteration and continue to the next iteration.
break - A loop-specific keyword that prompts the script to stop the current iteration and exit the loop entirely.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#keyword_version6)

//@version=6
indicator("`while` demo")

## [//@variable The number for which to calculate the factorial (N!).](https://www.tradingview.com/pine-script-reference/v6/#keyword_variableThenumberforwhichtocalculatethefactorialN.)

//@variable The number for which to calculate the factorial (N!).
//          The factorial is the product of all integers from 1 to `n`, with the exception (0! == 1).
int n = input.int(10, "N", 0)

## [//@variable The current value to multiply in the factorial calculation.](https://www.tradingview.com/pine-script-reference/v6/#keyword_variableThecurrentvaluetomultiplyinthefactorialcalculation.)

//@variable The current value to multiply in the factorial calculation.
var int counter = n
//@variable The factorial value.
var int factorial = 1

## [if barstate.isfirst](https://www.tradingview.com/pine-script-reference/v6/#keyword_ifbarstate.isfirst)

if barstate.isfirst
    // Repeatedly multiply `factorial` by `counter` and decrease the `counter` value by 1.
    // The loop ends after the value of `counter` becomes 0.
    while counter > 0
        factorial *= counter
        counter   -= 1

## [plot(factorial, "N!")](https://www.tradingview.com/pine-script-reference/v6/#keyword_plotfactorialN)

plot(factorial, "N!")
Remarks
If a script initializes a variable declared with var or varip using a loop's result, the loop stops after the first iteration, even if the header's criteria allow more iterations. Instead of initializing the variable with the result of this structure directly, declare the variable first and use := to update it with the result. Alternatively, move the loop into a Declaring functions and initialize the variable using a call to that function.
See the Loops page of our User Manual to learn more about loops and how they work.
