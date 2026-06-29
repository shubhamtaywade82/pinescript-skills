# Pine Script® v6 Type Reference

Source: https://www.tradingview.com/pine-script-reference/v6/

## [array](https://www.tradingview.com/pine-script-reference/v6/#type_array)

array

## [Keyword used to explicitly declare the "array" type of a variable or a parameter. Array objects (or IDs) can be created with the array.new<type>(), array.from() function.](https://www.tradingview.com/pine-script-reference/v6/#type_Keywordusedtoexplicitlydeclarethearraytypeofavariableoraparameter.ArrayobjectsorIDscanbecreatedwiththearray.newtypearray.fromfunction.)

Keyword used to explicitly declare the "array" type of a variable or a parameter. Array objects (or IDs) can be created with the array.new<type>(), array.from() function.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#type_version6)

//@version=6
indicator("array", overlay=true)
array<float> a = na
a := array.new<float>(1, close)
plot(array.get(a, 0))
Remarks
Array objects are always of "series" form.
See also
var
line
label
table
box
array.new<type>()
array.from()
bool

## [Keyword used to explicitly declare the "bool" (boolean) type of a variable or a parameter. "Bool" variables can have values true or false.](https://www.tradingview.com/pine-script-reference/v6/#type_Keywordusedtoexplicitlydeclaretheboolbooleantypeofavariableoraparameter.Boolvariablescanhavevaluestrueorfalse.)

Keyword used to explicitly declare the "bool" (boolean) type of a variable or a parameter. "Bool" variables can have values true or false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#type_version6)

//@version=6
indicator("bool")
bool b = true    // Same as `b = true`
plot(b ? open : close)
Remarks
Explicitly mentioning the type in a variable declaration is optional. Learn more about Pine Script® types in the User Manual page on the Type System.
See also
var
varip
int
float
color
string
true
false
box

## [Keyword used to explicitly declare the "box" type of a variable or a parameter. Box objects (or IDs) can be created with the box.new() function.](https://www.tradingview.com/pine-script-reference/v6/#type_Keywordusedtoexplicitlydeclaretheboxtypeofavariableoraparameter.BoxobjectsorIDscanbecreatedwiththebox.newfunction.)

Keyword used to explicitly declare the "box" type of a variable or a parameter. Box objects (or IDs) can be created with the box.new() function.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#type_version6)

//@version=6
indicator("box")
// Empty `box1` box ID.
var box box1 = na
// `box` type is unnecessary because `box.new()` returns a "box" type.
var box2 = box.new(na, na, na, na)
box3 = box.new(time, open, time + 60 *60* 24, close, xloc=xloc.bar_time)
Remarks
Box objects are always of "series" form.
See also
var
line
label
table
box.new()
chart.point

## [Keyword to explicitly declare the type of a variable or parameter as chart.point. Scripts can produce chart.point instances using the chart.point.from_time(), chart.point.from_index(), chart.point.now(), and chart.point.new() functions.](https://www.tradingview.com/pine-script-reference/v6/#type_Keywordtoexplicitlydeclarethetypeofavariableorparameteraschart.point.Scriptscanproducechart.pointinstancesusingthechart.point.from_timechart.point.from_indexchart.point.nowandchart.point.newfunctions.)

Keyword to explicitly declare the type of a variable or parameter as chart.point. Scripts can produce chart.point instances using the chart.point.from_time(), chart.point.from_index(), chart.point.now(), and chart.point.new() functions.
Fields
index (series int) The x-coordinate of the point, expressed as a bar index value.
time (series int) The x-coordinate of the point, expressed as a UNIX time value, in milliseconds.
price (series float) The y-coordinate of the point.
See also
polyline
color

## [Keyword used to explicitly declare the "color" type of a variable or a parameter.](https://www.tradingview.com/pine-script-reference/v6/#type_Keywordusedtoexplicitlydeclarethecolortypeofavariableoraparameter.)

Keyword used to explicitly declare the "color" type of a variable or a parameter.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#type_version6)

//@version=6
indicator("color", overlay = true)

## [color textColor = color.green](https://www.tradingview.com/pine-script-reference/v6/#type_colortextColorcolor.green)

color textColor = color.green
color labelColor = #FF000080 // Red color (FF0000) with 50% transparency (80 which is half of FF).
if barstate.islastconfirmedhistory
    label.new(bar_index, high, text = "Label", color = labelColor, textcolor = textColor)

## [// When declaring variables with color literals, built-in constants(color.green) or functions (color.new(), color.rgb()), the "color" keyword for the type can be omitted.](https://www.tradingview.com/pine-script-reference/v6/#type_Whendeclaringvariableswithcolorliteralsbuilt-inconstantscolor.greenorfunctionscolor.newcolor.rgbthecolorkeywordforthetypecanbeomitted.)

// When declaring variables with color literals, built-in constants(color.green) or functions (color.new(), color.rgb()), the "color" keyword for the type can be omitted.
c = color.rgb(0,255,0,0)
plot(close, color = c)
Remarks
Color literals have the following format: #RRGGBB or #RRGGBBAA. The letter pairs represent 00 to FF hexadecimal values (0 to 255 in decimal) where RR, GG and BB pairs are the values for the color's red, green and blue components. AA is an optional value for the color's transparency (or alpha component) where 00 is invisible and FF opaque. When no AA pair is supplied, FF is used. The hexadecimal letters can be upper or lower case.
Explicitly mentioning the type in a variable declaration is optional, except when it is initialized with na. Learn more about Pine Script® types in the User Manual page on the Type System.
See also
var
varip
int
float
string
color.rgb()
color.new()
const

## [The const keyword explicitly assigns the "const" type qualifier to variables and the parameters of non-exported functions.](https://www.tradingview.com/pine-script-reference/v6/#type_Theconstkeywordexplicitlyassignstheconsttypequalifiertovariablesandtheparametersofnon-exportedfunctions.)

The const keyword explicitly assigns the "const" type qualifier to variables and the parameters of non-exported functions.
