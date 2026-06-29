# Pine Script® v6 Functions Reference

Source: https://www.tradingview.com/pine-script-reference/v6/

## [alert()](https://www.tradingview.com/pine-script-reference/v6/#fun_alert)

alert()

## [Creates an alert trigger for an indicator or strategy, with a specified frequency, when called on the latest realtime bar. To activate alerts for a script containing calls to this function, open the "Create Alert" dialog box, then select the script name and "Any alert() function call" in the "Condition" section.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createsanalerttriggerforanindicatororstrategywithaspecifiedfrequencywhencalledonthelatestrealtimebar.ToactivatealertsforascriptcontainingcallstothisfunctionopentheCreateAlertdialogboxthenselectthescriptnameandAnyalertfunctioncallintheConditionsection.)

Creates an alert trigger for an indicator or strategy, with a specified frequency, when called on the latest realtime bar. To activate alerts for a script containing calls to this function, open the "Create Alert" dialog box, then select the script name and "Any alert() function call" in the "Condition" section.
Syntax
alert(message, freq) → void
Arguments
message (series string) The message to send when the alert occurs.
freq (input string) Optional. Determines the allowed frequency of the alert trigger. Possible values are: alert.freq_all (allows an alert on any realtime update), alert.freq_once_per_bar (allows an alert only on the first execution for each realtime bar), or alert.freq_once_per_bar_close (allows an alert only when a realtime bar closes). The default is alert.freq_once_per_bar.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`alert()` example", "", true)
ma = ta.sma(close, 14)
xUp = ta.crossover(close, ma)
if xUp
    // Trigger the alert the first time a cross occurs during the real-time bar.
    alert("Price (" + str.tostring(close) + ") crossed over MA (" + str.tostring(ma) + ").", alert.freq_once_per_bar)
plot(ma)
plotchar(xUp, "xUp", "▲", location.top, size = size.tiny)
Remarks
The alert() function does not display information on the chart.
In contrast to alertcondition(), calls to this function do not count toward a script's plot count. Additionally, alert() calls are allowed in local scopes, including the scopes of exported library functions.
See this article in our Help Center to learn more about activating alerts from alert() calls.
See also
alertcondition()
alertcondition()

## [Creates alert condition, that is available in Create Alert dialog. Please note, that alertcondition() does NOT create an alert, it just gives you more options in Create Alert dialog. Also, alertcondition() effect is invisible on chart.](https://www.tradingview.com/pine-script-reference/v6/#fun_CreatesalertconditionthatisavailableinCreateAlertdialog.PleasenotethatalertconditiondoesNOTcreateanalertitjustgivesyoumoreoptionsinCreateAlertdialog.Alsoalertconditioneffectisinvisibleonchart.)

Creates alert condition, that is available in Create Alert dialog. Please note, that alertcondition() does NOT create an alert, it just gives you more options in Create Alert dialog. Also, alertcondition() effect is invisible on chart.
Syntax
alertcondition(condition, title, message) → void
Arguments
condition (series bool) Series of boolean values that is used for alert. True values mean alert fire, false - no alert. Required argument.
title (const string) Title of the alert condition. Optional argument.
message (const string) Message to display when alert fires. Optional argument.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("alertcondition", overlay=true)
alertcondition(close >= open, title='Alert on Green Bar', message='Green Bar!')
Remarks
Please note that an alertcondition call generates an additional plot. All such calls are taken into account when we calculate the number of the output series per script.
See also
alert()
array.abs()2 overloads

## [Returns an array containing the absolute value of each element in the original array.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsanarraycontainingtheabsolutevalueofeachelementintheoriginalarray.)

Returns an array containing the absolute value of each element in the original array.
Syntax & Overloads
array.abs(id) → array<float>
array.abs(id) → array<int>
Arguments
id (array<int/float>) An array object.
See also
array.new_float()
array.insert()
array.slice()
array.reverse()
order.ascending
order.descending
array.avg()2 overloads

## [The function returns the mean of an array's elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthemeanofanarrayselements.)

The function returns the mean of an array's elements.
Syntax & Overloads
array.avg(id) → series float
array.avg(id) → series int
Arguments
id (array<int/float>) An array object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.avg example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.avg(a))
Returns
Mean of array's elements.
Remarks
Returns na if the id array is empty.
See also
array.new_float()
array.max()
array.min()
array.stdev()
array.binary_search()

## [The function returns the index of the value, or -1 if the value is not found. The array to search must be sorted in ascending order.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnstheindexofthevalueor-1ifthevalueisnotfound.Thearraytosearchmustbesortedinascendingorder.)

The function returns the index of the value, or -1 if the value is not found. The array to search must be sorted in ascending order.
Syntax
array.binary_search(id, val) → series int
Arguments
id (array<int/float>) An array object.
val (series int/float) The value to search for in the array.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.binary_search")
a = array.from(5, -2, 0, 9, 1)
array.sort(a) // [-2, 0, 1, 5, 9]
position = array.binary_search(a, 0) // 1
plot(position)
Remarks
A binary search works on arrays pre-sorted in ascending order. It begins by comparing an element in the middle of the array with the target value. If the element matches the target value, its position in the array is returned. If the element's value is greater than the target value, the search continues in the lower half of the array. If the element's value is less than the target value, the search continues in the upper half of the array. By doing this recursively, the algorithm progressively eliminates smaller and smaller portions of the array in which the target value cannot lie.
See also
array.new_float()
array.insert()
array.slice()
array.reverse()
order.ascending
order.descending
array.binary_search_leftmost()

## [The function returns the index of the value if it is found. When the value is not found, the function returns the index of the next smallest element to the left of where the value would lie if it was in the array. The array to search must be sorted in ascending order.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnstheindexofthevalueifitisfound.Whenthevalueisnotfoundthefunctionreturnstheindexofthenextsmallestelementtotheleftofwherethevaluewouldlieifitwasinthearray.Thearraytosearchmustbesortedinascendingorder.)

The function returns the index of the value if it is found. When the value is not found, the function returns the index of the next smallest element to the left of where the value would lie if it was in the array. The array to search must be sorted in ascending order.
Syntax
array.binary_search_leftmost(id, val) → series int
Arguments
id (array<int/float>) An array object.
val (series int/float) The value to search for in the array.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.binary_search_leftmost")
a = array.from(5, -2, 0, 9, 1)
array.sort(a) // [-2, 0, 1, 5, 9]
position = array.binary_search_leftmost(a, 3) // 2
plot(position)
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.binary_search_leftmost, repetitive elements")
a = array.from(4, 5, 5, 5)
// Returns the index of the first instance.
position = array.binary_search_leftmost(a, 5)
plot(position) // Plots 1
Remarks
A binary search works on arrays pre-sorted in ascending order. It begins by comparing an element in the middle of the array with the target value. If the element matches the target value, its position in the array is returned. If the element's value is greater than the target value, the search continues in the lower half of the array. If the element's value is less than the target value, the search continues in the upper half of the array. By doing this recursively, the algorithm progressively eliminates smaller and smaller portions of the array in which the target value cannot lie.
See also
array.new_float()
array.insert()
array.slice()
array.reverse()
order.ascending
order.descending
array.binary_search_rightmost()

## [The function returns the index of the value if it is found. When the value is not found, the function returns the index of the element to the right of where the value would lie if it was in the array. The array must be sorted in ascending order.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnstheindexofthevalueifitisfound.Whenthevalueisnotfoundthefunctionreturnstheindexoftheelementtotherightofwherethevaluewouldlieifitwasinthearray.Thearraymustbesortedinascendingorder.)

The function returns the index of the value if it is found. When the value is not found, the function returns the index of the element to the right of where the value would lie if it was in the array. The array must be sorted in ascending order.
Syntax
array.binary_search_rightmost(id, val) → series int
Arguments
id (array<int/float>) An array object.
val (series int/float) The value to search for in the array.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.binary_search_rightmost")
a = array.from(5, -2, 0, 9, 1)
array.sort(a) // [-2, 0, 1, 5, 9]
position = array.binary_search_rightmost(a, 3) // 3
plot(position)
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.binary_search_rightmost, repetitive elements")
a = array.from(4, 5, 5, 5)
// Returns the index of the last instance.
position = array.binary_search_rightmost(a, 5)
plot(position) // Plots 3
Remarks
A binary search works on sorted arrays in ascending order. It begins by comparing an element in the middle of the array with the target value. If the element matches the target value, its position in the array is returned. If the element's value is greater than the target value, the search continues in the lower half of the array. If the element's value is less than the target value, the search continues in the upper half of the array. By doing this recursively, the algorithm progressively eliminates smaller and smaller portions of the array in which the target value cannot lie.
See also
array.new_float()
array.insert()
array.slice()
array.reverse()
order.ascending
order.descending
array.clear()

## [The function removes all elements from an array.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionremovesallelementsfromanarray.)

The function removes all elements from an array.
Syntax
array.clear(id) → void
Arguments
id (any array type) An array object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.clear example")
a = array.new_float(5,high)
array.clear(a)
array.push(a, close)
plot(array.get(a,0))
plot(array.size(a))
See also
array.new_float()
array.insert()
array.push()
array.remove()
array.pop()
array.concat()

## [The function is used to merge two arrays. It pushes all elements from the second array to the first array, and returns the first array.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionisusedtomergetwoarrays.Itpushesallelementsfromthesecondarraytothefirstarrayandreturnsthefirstarray.)

The function is used to merge two arrays. It pushes all elements from the second array to the first array, and returns the first array.
Syntax
array.concat(id1, id2) → array<type>
Arguments
id1 (any array type) The first array object.
id2 (any array type) The second array object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.concat example")
a = array.new_float(0,0)
b = array.new_float(0,0)
for i = 0 to 4
    array.push(a, high[i])
    array.push(b, low[i])
c = array.concat(a,b)
plot(array.size(a))
plot(array.size(b))
plot(array.size(c))
Returns
The first array with merged elements from the second array.
See also
array.new_float()
array.insert()
array.slice()
array.copy()

## [The function creates a copy of an existing array.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncreatesacopyofanexistingarray.)

The function creates a copy of an existing array.
Syntax
array.copy(id) → array<type>
Arguments
id (any array type) An array object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.copy example")
length = 5
a = array.new_float(length, close)
b = array.copy(a)
a := array.new_float(length, open)
plot(array.sum(a) / length)
plot(array.sum(b) / length)
Returns
A copy of an array.
See also
array.new_float()
array.get()
array.slice()
array.sort()
array.covariance()

## [The function returns the covariance of two arrays.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthecovarianceoftwoarrays.)

The function returns the covariance of two arrays.
Syntax
array.covariance(id1, id2, biased) → series float
Arguments
id1 (array<int/float>) An array object.
id2 (array<int/float>) An array object.
biased (series bool) Determines which estimate should be used. Optional. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.covariance example")
a = array.new_float(0)
b = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
    array.push(b, open[i])
plot(array.covariance(a, b))
Returns
The covariance of two arrays.
Remarks
If biased is true, function will calculate using a biased estimate of the entire population, if false - unbiased estimate of a sample. Returns na if both arrays are empty.
See also
array.new_float()
array.max()
array.stdev()
array.avg()
array.variance()
array.every()

## [Returns true if all elements of the id array are true, false otherwise.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnstrueifallelementsoftheidarrayaretruefalseotherwise.)

Returns true if all elements of the id array are true, false otherwise.
Syntax
array.every(id) → series bool
Arguments
id (array<bool>) An array object.
Remarks
This function also works with arrays of int and float types, in which case zero values are considered false, and all others true.
See also
array.some()
array.get()
array.fill()

## [The function sets elements of an array to a single value. If no index is specified, all elements are set. If only a start index (default 0) is supplied, the elements starting at that index are set. If both index parameters are used, the elements from the starting index up to but not including the end index (default na) are set.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionsetselementsofanarraytoasinglevalue.Ifnoindexisspecifiedallelementsareset.Ifonlyastartindexdefault0issuppliedtheelementsstartingatthatindexareset.Ifbothindexparametersareusedtheelementsfromthestartingindexuptobutnotincludingtheendindexdefaultnaareset.)

The function sets elements of an array to a single value. If no index is specified, all elements are set. If only a start index (default 0) is supplied, the elements starting at that index are set. If both index parameters are used, the elements from the starting index up to but not including the end index (default na) are set.
Syntax
array.fill(id, value, index_from, index_to) → void
Arguments
id (any array type) An array object.
value (series <type of the array's elements>) Value to fill the array with.
index_from (series int) Start index, default is 0.
index_to (series int) End index, default is na. Must be one greater than the index of the last element to set.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.fill example")
a = array.new_float(10)
array.fill(a, close)
plot(array.sum(a))
See also
array.new_float()
array.set()
array.slice()
array.first()

## [Returns the array's first element. Throws a runtime error if the array is empty.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthearraysfirstelement.Throwsaruntimeerrorifthearrayisempty.)

Returns the array's first element. Throws a runtime error if the array is empty.
Syntax
array.first(id) → series <type>
Arguments
id (any array type) An array object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.first example")
arr = array.new_int(3, 10)
plot(array.first(arr))
See also
array.last()
array.get()
array.from()12 overloads

## [The function takes a variable number of arguments with one of the types: int, float, bool, string, label, line, color, box, table, linefill, and returns an array of the corresponding type.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctiontakesavariablenumberofargumentswithoneofthetypesintfloatboolstringlabellinecolorboxtablelinefillandreturnsanarrayofthecorrespondingtype.)

The function takes a variable number of arguments with one of the types: int, float, bool, string, label, line, color, box, table, linefill, and returns an array of the corresponding type.
Syntax & Overloads
array.from(arg0, arg1, ...) → array<type>
array.from(arg0, arg1, ...) → array<enum>
array.from(arg0, arg1, ...) → array<label>
array.from(arg0, arg1, ...) → array<line>
array.from(arg0, arg1, ...) → array<box>
array.from(arg0, arg1, ...) → array<table>
array.from(arg0, arg1, ...) → array<linefill>
array.from(arg0, arg1, ...) → array<string>
array.from(arg0, arg1, ...) → array<color>
array.from(arg0, arg1, ...) → array<int>
array.from(arg0, arg1, ...) → array<float>
array.from(arg0, arg1, ...) → array<bool>
Arguments
arg0, arg1, ... (<arg..._type>) Array arguments.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.from_example", overlay = false)
arr = array.from("Hello", "World!") // arr (array<string>) will contain 2 elements: {Hello}, {World!}.
plot(close)
Returns
The array element's value.
Remarks
This function can accept up to 4,000 'int', 'float', 'bool', or 'color' arguments. For all other types, including user-defined types, the limit is 999.
array.get()

## [The function returns the value of the element at the specified index.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthevalueoftheelementatthespecifiedindex.)

The function returns the value of the element at the specified index.
Syntax
array.get(id, index) → series <type>
Arguments
id (any array type) An array object.
index (series int) The index of the element whose value is to be returned.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.get example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i] - open[i])
plot(array.get(a, 9))
Returns
The array element's value.
Remarks
If the index is positive, the function counts forwards from the beginning of the array to the end. The index of the first element is 0, and the index of the last element is array.size() - 1. If the index is negative, the function counts backwards from the end of the array to the beginning. In this case, the index of the last element is -1, and the index of the first element is negative array.size(). For example, for an array that contains three elements, all of the following are valid arguments for the index parameter: 0, 1, 2, -1, -2, -3.
See also
array.new_float()
array.set()
array.slice()
array.sort()
array.includes()

## [The function returns true if the value was found in an array, false otherwise.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnstrueifthevaluewasfoundinanarrayfalseotherwise.)

The function returns true if the value was found in an array, false otherwise.
Syntax
array.includes(id, value) → series bool
Arguments
id (any array type) An array object.
value (series <type of the array's elements>) The value to search in the array.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.includes example")
a = array.new_float(5,high)
p = close
if array.includes(a, high)
    p := open
plot(p)
Returns
True if the value was found in the array, false otherwise.
See also
array.new_float()
array.indexof()
array.shift()
array.remove()
array.insert()
array.indexof()

## [The function returns the index of the first occurrence of the value, or -1 if the value is not found.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnstheindexofthefirstoccurrenceofthevalueor-1ifthevalueisnotfound.)

The function returns the index of the first occurrence of the value, or -1 if the value is not found.
Syntax
array.indexof(id, value) → series int
Arguments
id (any array type) An array object.
value (series <type of the array's elements>) The value to search in the array.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.indexof example")
a = array.new_float(5,high)
index = array.indexof(a, high)
plot(index)
Returns
The index of an element.
See also
array.lastindexof()
array.get()
array.lastindexof()
array.remove()
array.insert()
array.insert()

## [The function changes the contents of an array by adding new elements in place.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionchangesthecontentsofanarraybyaddingnewelementsinplace.)

The function changes the contents of an array by adding new elements in place.
Syntax
array.insert(id, index, value) → void
Arguments
id (any array type) An array object.
index (series int) The index at which to insert the value.
value (series <type of the array's elements>) The value to add to the array.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.insert example")
a = array.new_float(5, close)
array.insert(a, 0, open)
plot(array.get(a, 5))
Remarks
If the index is positive, the function counts forwards from the beginning of the array to the end. The index of the first element is 0, and the index of the last element is array.size() - 1. If the index is negative, the function counts backwards from the end of the array to the beginning. In this case, the index of the last element is -1, and the index of the first element is negative array.size(). For example, for an array that contains three elements, all of the following are valid arguments for the index parameter: 0, 1, 2, -1, -2, -3.
See also
array.new_float()
array.set()
array.push()
array.remove()
array.pop()
array.unshift()
array.join()

## [The function creates and returns a new string by concatenating all the elements of an array, separated by the specified separator string.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncreatesandreturnsanewstringbyconcatenatingalltheelementsofanarrayseparatedbythespecifiedseparatorstring.)

The function creates and returns a new string by concatenating all the elements of an array, separated by the specified separator string.
Syntax
array.join(id, separator) → series string
Arguments
id (array<int/float/string>) An array object.
separator (series string) The string used to separate each array element.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.join example")
a = array.new_float(5, 5)
label.new(bar_index, close, array.join(a, ","))
See also
array.new_float()
array.set()
array.insert()
array.remove()
array.pop()
array.unshift()
array.last()

## [Returns the array's last element. Throws a runtime error if the array is empty.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthearrayslastelement.Throwsaruntimeerrorifthearrayisempty.)

Returns the array's last element. Throws a runtime error if the array is empty.
Syntax
array.last(id) → series <type>
Arguments
id (any array type) An array object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.last example")
arr = array.new_int(3, 10)
plot(array.last(arr))
See also
array.first()
array.get()
array.lastindexof()

## [The function returns the index of the last occurrence of the value, or -1 if the value is not found.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnstheindexofthelastoccurrenceofthevalueor-1ifthevalueisnotfound.)

The function returns the index of the last occurrence of the value, or -1 if the value is not found.
Syntax
array.lastindexof(id, value) → series int
Arguments
id (any array type) An array object.
value (series <type of the array's elements>) The value to search in the array.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.lastindexof example")
a = array.new_float(5,high)
index = array.lastindexof(a, high)
plot(index)
Returns
The index of an element.
See also
array.new_float()
array.set()
array.push()
array.remove()
array.insert()
array.max()2 overloads

## [The function returns the greatest value, or the nth greatest value in a given array.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthegreatestvalueorthenthgreatestvalueinagivenarray.)

The function returns the greatest value, or the nth greatest value in a given array.
Syntax & Overloads
array.max(id, nth) → series float
array.max(id, nth) → series int
Arguments
id (array<int/float>) An array object.
nth (series int) The nth greatest value to return, where zero is the greatest. Optional. The default is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.max")
a = array.from(5, -2, 0, 9, 1)
thirdHighest = array.max(a, 2) // 1
plot(thirdHighest)
Returns
The greatest or the nth greatest value in the array.
Remarks
Returns na if the id array is empty.
See also
array.new_float()
array.min()
array.sum()
array.median()2 overloads

## [The function returns the median of an array's elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthemedianofanarrayselements.)

The function returns the median of an array's elements.
Syntax & Overloads
array.median(id) → series float
array.median(id) → series int
Arguments
id (array<int/float>) An array object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.median example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.median(a))
Returns
The median of the array's elements.
Remarks
Returns na if the id array is empty.
See also
array.median()
array.avg()
array.variance()
array.min()
array.min()2 overloads

## [The function returns the smallest value, or the nth smallest value in a given array.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthesmallestvalueorthenthsmallestvalueinagivenarray.)

The function returns the smallest value, or the nth smallest value in a given array.
Syntax & Overloads
array.min(id, nth) → series float
array.min(id, nth) → series int
Arguments
id (array<int/float>) An array object.
nth (series int) The nth smallest value to return, where zero is the smallest. Optional. The default is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.min")
a = array.from(5, -2, 0, 9, 1)
secondLowest = array.min(a, 1) // 0
plot(secondLowest)
Returns
The smallest or the nth smallest value in the array.
Remarks
Returns na if the id array is empty.
See also
array.new_float()
array.max()
array.sum()
array.mode()2 overloads

## [The function returns the mode of an array's elements. If there are several values with the same frequency, it returns the smallest value.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthemodeofanarrayselements.Ifthereareseveralvalueswiththesamefrequencyitreturnsthesmallestvalue.)

The function returns the mode of an array's elements. If there are several values with the same frequency, it returns the smallest value.
Syntax & Overloads
array.mode(id) → series float
array.mode(id) → series int
Arguments
id (array<int/float>) An array object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.mode example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.mode(a))
Returns
The most frequently occurring value from the id array. If none exists, returns the smallest value instead.
Remarks
Returns na if the id array is empty.
See also
array.new_float()
ta.mode()
matrix.mode()
array.avg()
array.variance()
array.min()
array.new_bool()

## [The function creates a new array object of bool type elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncreatesanewarrayobjectofbooltypeelements.)

The function creates a new array object of bool type elements.
Syntax
array.new_bool(size, initial_value) → array<bool>
Arguments
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series bool) Initial value of all array elements. Optional. The default is 'false'.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.new_bool example")
length = 5
a = array.new_bool(length, close > open)
plot(array.get(a, 0) ? close : open)
Returns
The ID of an array object which may be used in other array.*() functions.
Remarks
An array index starts from 0.
See also
array.new_float()
array.get()
array.slice()
array.sort()
array.new_box()

## [The function creates a new array object of box type elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncreatesanewarrayobjectofboxtypeelements.)

The function creates a new array object of box type elements.
Syntax
array.new_box(size, initial_value) → array<box>
Arguments
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series box) Initial value of all array elements. Optional. The default is 'na'.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.new_box example")
boxes = array.new_box()
array.push(boxes, box.new(time, close, time+2, low, xloc=xloc.bar_time))
plot(1)
Returns
The ID of an array object which may be used in other array.*() functions.
Remarks
An array index starts from 0.
See also
array.new_float()
array.get()
array.slice()
array.new_color()

## [The function creates a new array object of color type elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncreatesanewarrayobjectofcolortypeelements.)

The function creates a new array object of color type elements.
Syntax
array.new_color(size, initial_value) → array<color>
Arguments
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series color) Initial value of all array elements. Optional. The default is 'na'.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.new_color example")
length = 5
a = array.new_color(length, color.red)
plot(close, color = array.get(a, 0))
Returns
The ID of an array object which may be used in other array.*() functions.
Remarks
An array index starts from 0.
See also
array.new_float()
array.get()
array.slice()
array.sort()
array.new_float()

## [The function creates a new array object of float type elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncreatesanewarrayobjectoffloattypeelements.)

The function creates a new array object of float type elements.
Syntax
array.new_float(size, initial_value) → array<float>
Arguments
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series int/float) Initial value of all array elements. Optional. The default is 'na'.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.new_float example")
length = 5
a = array.new_float(length, close)
plot(array.sum(a) / length)
Returns
The ID of an array object which may be used in other array.*() functions.
Remarks
An array index starts from 0.
See also
array.new_color()
array.new_bool()
array.get()
array.slice()
array.sort()
array.new_int()

## [The function creates a new array object of int type elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncreatesanewarrayobjectofinttypeelements.)

The function creates a new array object of int type elements.
Syntax
array.new_int(size, initial_value) → array<int>
Arguments
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series int) Initial value of all array elements. Optional. The default is 'na'.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.new_int example")
length = 5
a = array.new_int(length, int(close))
plot(array.sum(a) / length)
Returns
The ID of an array object which may be used in other array.*() functions.
Remarks
An array index starts from 0.
See also
array.new_float()
array.get()
array.slice()
array.sort()
array.new_label()

## [The function creates a new array object of label type elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncreatesanewarrayobjectoflabeltypeelements.)

The function creates a new array object of label type elements.
Syntax
array.new_label(size, initial_value) → array<label>
Arguments
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series label) Initial value of all array elements. Optional. The default is 'na'.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.new_label example", overlay = true, max_labels_count = 500)

## [//@variable The number of labels to show on the chart.](https://www.tradingview.com/pine-script-reference/v6/#fun_variableThenumberoflabelstoshowonthechart.)

//@variable The number of labels to show on the chart.
int labelCount = input.int(50, "Labels to show", 1, 500)

## [//@variable An array of `label` objects.](https://www.tradingview.com/pine-script-reference/v6/#fun_variableAnarrayoflabelobjects.)

//@variable An array of `label` objects.
var array<label> labelArray = array.new_label()

## [//@variable A `chart.point` for the new label.](https://www.tradingview.com/pine-script-reference/v6/#fun_variableAchart.pointforthenewlabel.)

//@variable A `chart.point` for the new label.
labelPoint = chart.point.from_index(bar_index, close)
//@variable The text in the new label.
string labelText = na
//@variable The color of the new label.
color labelColor = na
//@variable The style of the new label.
string labelStyle = na

## [// Set the label attributes for rising bars.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setthelabelattributesforrisingbars.)

// Set the label attributes for rising bars.
if close > open
    labelText  := "Rising"
    labelColor := color.green
    labelStyle := label.style_label_down
// Set the label attributes for falling bars.
else if close < open
    labelText  := "Falling"
    labelColor := color.red
    labelStyle := label.style_label_up

## [// Add a new label to the `labelArray` when the chart bar closed at a new value.](https://www.tradingview.com/pine-script-reference/v6/#fun_AddanewlabeltothelabelArraywhenthechartbarclosedatanewvalue.)

// Add a new label to the `labelArray` when the chart bar closed at a new value.
if close != open
    labelArray.push(label.new(labelPoint, labelText, color = labelColor, style = labelStyle))
// Remove the first element and delete its label when the size of the `labelArray` exceeds the `labelCount`.
if labelArray.size() > labelCount
    label.delete(labelArray.shift())
Returns
The ID of an array object which may be used in other array.*() functions.
Remarks
An array index starts from 0.
See also
array.new_float()
array.get()
array.slice()
array.new_line()

## [The function creates a new array object of line type elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncreatesanewarrayobjectoflinetypeelements.)

The function creates a new array object of line type elements.
Syntax
array.new_line(size, initial_value) → array<line>
Arguments
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series line) Initial value of all array elements. Optional. The default is 'na'.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.new_line example")
// draw last 15 lines
var a = array.new_line()
array.push(a, line.new(bar_index - 1, close[1], bar_index, close))
if array.size(a) > 15
    ln = array.shift(a)
    line.delete(ln)
Returns
The ID of an array object which may be used in other array.*() functions.
Remarks
An array index starts from 0.
See also
array.new_float()
array.get()
array.slice()
array.new_linefill()

## [The function creates a new array object of linefill type elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncreatesanewarrayobjectoflinefilltypeelements.)

The function creates a new array object of linefill type elements.
Syntax
array.new_linefill(size, initial_value) → array<linefill>
Arguments
size (series int) Initial size of an array.
initial_value (series linefill) Initial value of all array elements.
Returns
The ID of an array object which may be used in other array.*() functions.
Remarks
An array index starts from 0.
array.new_string()

## [The function creates a new array object of string type elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncreatesanewarrayobjectofstringtypeelements.)

The function creates a new array object of string type elements.
Syntax
array.new_string(size, initial_value) → array<string>
Arguments
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series string) Initial value of all array elements. Optional. The default is 'na'.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.new_string example")
length = 5
a = array.new_string(length, "text")
label.new(bar_index, close, array.get(a, 0))
Returns
The ID of an array object which may be used in other array.*() functions.
Remarks
An array index starts from 0.
See also
array.new_float()
array.get()
array.slice()
array.new_table()

## [The function creates a new array object of table type elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncreatesanewarrayobjectoftabletypeelements.)

The function creates a new array object of table type elements.
Syntax
array.new_table(size, initial_value) → array<table>
Arguments
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (series table) Initial value of all array elements. Optional. The default is 'na'.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("table array")
tables = array.new_table()
array.push(tables, table.new(position = position.top_left, rows = 1, columns = 2, bgcolor = color.yellow, border_width=1))
plot(1)
Returns
The ID of an array object which may be used in other array.*() functions.
Remarks
An array index starts from 0.
See also
array.new_float()
array.get()
array.slice()
array.new<type>()

## [The function creates a new array object of <type> elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncreatesanewarrayobjectoftypeelements.)

The function creates a new array object of <type> elements.
Syntax
array.new<type>(size, initial_value) → array<type>
Arguments
size (series int) Initial size of an array. Optional. The default is 0.
initial_value (<array_type>) Initial value of all array elements. Optional. The default is 'na'.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.new<string> example")
a = array.new<string>(1, "Hello, World!")
label.new(bar_index, close, array.get(a, 0))
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.new<color> example")
a = array.new<color>()
array.push(a, color.red)
array.push(a, color.green)
plot(close, color = array.get(a, close > open ? 1 : 0))
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.new<float> example")
length = 5
var a = array.new<float>(length, close)
if array.size(a) == length
    array.remove(a, 0)
    array.push(a, close)
plot(array.sum(a) / length, "SMA")
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.new<line> example")
// draw last 15 lines
var a = array.new<line>()
array.push(a, line.new(bar_index - 1, close[1], bar_index, close))
if array.size(a) > 15
    ln = array.shift(a)
    line.delete(ln)
Returns
The ID of an array object which may be used in other array.*() functions.
Remarks
An array index starts from 0.
If you want to initialize an array and specify all its elements at the same time, then use the function array.from.
See also
array.from()
array.push()
array.get()
array.size()
array.remove()
array.shift()
array.sum()
array.percentile_linear_interpolation()2 overloads

## [Returns the value for which the specified percentage of array values (percentile) are less than or equal to it, using linear interpolation.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthevalueforwhichthespecifiedpercentageofarrayvaluespercentilearelessthanorequaltoitusinglinearinterpolation.)

Returns the value for which the specified percentage of array values (percentile) are less than or equal to it, using linear interpolation.
Syntax & Overloads
array.percentile_linear_interpolation(id, percentage) → series float
array.percentile_linear_interpolation(id, percentage) → series int
Arguments
id (array<int/float>) An array object.
percentage (series int/float) The percentage of values that must be equal or less than the returned value.
Remarks
In statistics, the percentile is the percent of ranking items that appear at or below a certain score. This measurement shows the percentage of scores within a standard frequency distribution that is lower than the percentile rank being measured. Linear interpolation estimates the value between two ranks.
Returns na if the id array is empty.
See also
array.new_float()
array.insert()
array.slice()
array.reverse()
order.ascending
order.descending
array.percentile_nearest_rank()2 overloads

## [Returns the value for which the specified percentage of array values (percentile) are less than or equal to it, using the nearest-rank method.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthevalueforwhichthespecifiedpercentageofarrayvaluespercentilearelessthanorequaltoitusingthenearest-rankmethod.)

Returns the value for which the specified percentage of array values (percentile) are less than or equal to it, using the nearest-rank method.
Syntax & Overloads
array.percentile_nearest_rank(id, percentage) → series float
array.percentile_nearest_rank(id, percentage) → series int
Arguments
id (array<int/float>) An array object.
percentage (series int/float) The percentage of values that must be equal or less than the returned value.
Remarks
In statistics, the percentile is the percent of ranking items that appear at or below a certain score. This measurement shows the percentage of scores within a standard frequency distribution that is lower than the percentile rank you're measuring.
Returns na if the id array is empty.
See also
array.new_float()
array.insert()
array.slice()
array.reverse()
order.ascending
order.descending
array.percentrank()2 overloads

## [Returns the percentile rank of the element at the specified index.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthepercentilerankoftheelementatthespecifiedindex.)

Returns the percentile rank of the element at the specified index.
Syntax & Overloads
array.percentrank(id, index) → series float
array.percentrank(id, index) → series int
Arguments
id (array<int/float>) An array object.
index (series int) The index of the element for which the percentile rank should be calculated.
Remarks
Percentile rank is the number of elements in the array that are less than or equal to the reference value, expressed as a percentage.
Returns na if the id array is empty.
See also
array.new_float()
array.insert()
array.slice()
array.reverse()
order.ascending
order.descending

## [array.pop()](https://www.tradingview.com/pine-script-reference/v6/#fun_array.pop)

array.pop()

## [The function removes the last element from an array and returns its value.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionremovesthelastelementfromanarrayandreturnsitsvalue.)

The function removes the last element from an array and returns its value.
Syntax
array.pop(id) → series <type>
Arguments
id (any array type) An array object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.pop example")
a = array.new_float(5,high)
removedEl = array.pop(a)
plot(array.size(a))
plot(removedEl)
Returns
The value of the removed element.
See also
array.new_float()
array.set()
array.push()
array.remove()
array.insert()
array.shift()
array.push()

## [The function appends a value to an array.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionappendsavaluetoanarray.)

The function appends a value to an array.
Syntax
array.push(id, value) → void
Arguments
id (any array type) An array object.
value (series <type of the array's elements>) The value of the element added to the end of the array.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.push example")
a = array.new_float(5, 0)
array.push(a, open)
plot(array.get(a, 5))
See also
array.new_float()
array.set()
array.insert()
array.remove()
array.pop()
array.unshift()
array.range()2 overloads

## [The function returns the difference between the min and max values from a given array.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthedifferencebetweentheminandmaxvaluesfromagivenarray.)

The function returns the difference between the min and max values from a given array.
Syntax & Overloads
array.range(id) → series float
array.range(id) → series int
Arguments
id (array<int/float>) An array object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.range example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.range(a))
Returns
The difference between the min and max values in the array.
Remarks
Returns na if the id array is empty.
See also
array.new_float()
array.min()
array.max()
array.sum()
array.remove()

## [The function changes the contents of an array by removing the element with the specified index.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionchangesthecontentsofanarraybyremovingtheelementwiththespecifiedindex.)

The function changes the contents of an array by removing the element with the specified index.
Syntax
array.remove(id, index) → series <type>
Arguments
id (any array type) An array object.
index (series int) The index of the element to remove.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.remove example")
a = array.new_float(5,high)
removedEl = array.remove(a, 0)
plot(array.size(a))
plot(removedEl)
Returns
The value of the removed element.
Remarks
If the index is positive, the function counts forwards from the beginning of the array to the end. The index of the first element is 0, and the index of the last element is array.size() - 1. If the index is negative, the function counts backwards from the end of the array to the beginning. In this case, the index of the last element is -1, and the index of the first element is negative array.size(). For example, for an array that contains three elements, all of the following are valid arguments for the index parameter: 0, 1, 2, -1, -2, -3.
See also
array.new_float()
array.set()
array.push()
array.insert()
array.pop()
array.shift()
array.reverse()

## [The function reverses an array. The first array element becomes the last, and the last array element becomes the first.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreversesanarray.Thefirstarrayelementbecomesthelastandthelastarrayelementbecomesthefirst.)

The function reverses an array. The first array element becomes the last, and the last array element becomes the first.
Syntax
array.reverse(id) → void
Arguments
id (any array type) An array object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.reverse example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.get(a, 0))
array.reverse(a)
plot(array.get(a, 0))
See also
array.new_float()
array.sort()
array.push()
array.set()
array.avg()
array.set()

## [The function sets the value of the element at the specified index.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionsetsthevalueoftheelementatthespecifiedindex.)

The function sets the value of the element at the specified index.
Syntax
array.set(id, index, value) → void
Arguments
id (any array type) An array object.
index (series int) The index of the element to be modified.
value (series <type of the array's elements>) The new value to be set.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.set example")
a = array.new_float(10)
for i = 0 to 9
    array.set(a, i, close[i])
plot(array.sum(a) / 10)
Remarks
If the index is positive, the function counts forwards from the beginning of the array to the end. The index of the first element is 0, and the index of the last element is array.size() - 1. If the index is negative, the function counts backwards from the end of the array to the beginning. In this case, the index of the last element is -1, and the index of the first element is negative array.size(). For example, for an array that contains three elements, all of the following are valid arguments for the index parameter: 0, 1, 2, -1, -2, -3.
See also
array.new_float()
array.get()
array.slice()
array.shift()

## [The function removes an array's first element and returns its value.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionremovesanarraysfirstelementandreturnsitsvalue.)

The function removes an array's first element and returns its value.
Syntax
array.shift(id) → series <type>
Arguments
id (any array type) An array object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.shift example")
a = array.new_float(5,high)
removedEl = array.shift(a)
plot(array.size(a))
plot(removedEl)
Returns
The value of the removed element.
See also
array.unshift()
array.set()
array.push()
array.remove()
array.includes()
array.size()

## [The function returns the number of elements in an array.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthenumberofelementsinanarray.)

The function returns the number of elements in an array.
Syntax
array.size(id) → series int
Arguments
id (any array type) An array object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.size example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
// note that changes in slice also modify original array
slice = array.slice(a, 0, 5)
array.push(slice, open)
// size was changed in slice and in original array
plot(array.size(a))
plot(array.size(slice))
Returns
The number of elements in the array.
See also
array.new_float()
array.sum()
array.slice()
array.sort()
array.slice()

## [Creates an array representing a slice of an existing array. Setting a slice's element to a new value changes the corresponding element in the original array to that value. Likewise, inserting or removing an element in the slice inserts or removes an element in the original array at the index range covered by the slice.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createsanarrayrepresentingasliceofanexistingarray.Settingasliceselementtoanewvaluechangesthecorrespondingelementintheoriginalarraytothatvalue.Likewiseinsertingorremovinganelementinthesliceinsertsorremovesanelementintheoriginalarrayattheindexrangecoveredbytheslice.)

Creates an array representing a slice of an existing array. Setting a slice's element to a new value changes the corresponding element in the original array to that value. Likewise, inserting or removing an element in the slice inserts or removes an element in the original array at the index range covered by the slice.
Syntax
array.slice(id, index_from, index_to) → array<type>
Arguments
id (any array type) The reference (ID) of the array from which to create a new slice.
index_from (series int) The id array index corresponding to the start of the slice.
index_to (series int) The id array index corresponding to the end of the slice. The index is non-inclusive; the resulting slice contains all the original array's elements from index_from to index_to - 1.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.slice example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
// take elements from 0 to 4
// *note that changes in slice also modify original array
slice = array.slice(a, 0, 5)
plot(array.sum(a) / 10)
plot(array.sum(slice) / 5)
Returns
The ID of an array representing a slice of the id array.
Remarks
The indices in the resulting slice range from zero to one less than the slice's size. These indices do not directly represent the same element indices as the original array. For example, if the index_from value is 5, the slice's element at index 1 refers to the id array's element at index 6.
Scripts cannot modify the elements of a historical array. Therefore, they cannot modify historical array slices created by this function. Instead of modifying an array referenced by an ID retrieved with the [] operator, use array.copy() to create a shallow copy of the historical array, then modify the copy or a slice of that copy instead.
See also
array.new_float()
array.get()
array.sort()
array.some()

## [Returns true if at least one element of the id array is true, false otherwise.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnstrueifatleastoneelementoftheidarrayistruefalseotherwise.)

Returns true if at least one element of the id array is true, false otherwise.
Syntax
array.some(id) → series bool
Arguments
id (array<bool>) An array object.
Remarks
This function also works with arrays of int and float types, in which case zero values are considered false, and all others true.
See also
array.every()
array.get()
array.sort()2 overloads

## [The function sorts the elements of an array.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionsortstheelementsofanarray.)

The function sorts the elements of an array.
Syntax & Overloads
array.sort(id, order) → void
array.sort(id, order, sort_field) → void
Arguments
id (array<int/float/string>) An array object.
order (series sort_order) The sort order: order.ascending (default) or order.descending.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.sort example")
a = array.new_float(0,0)
for i = 0 to 5
    array.push(a, high[i])
array.sort(a, order.descending)
if barstate.islast
    label.new(bar_index, close, str.tostring(a))
See also
array.new_float()
array.insert()
array.slice()
array.reverse()
order.ascending
order.descending
array.sort_indices()2 overloads

## [Returns an array of indices which, when used to index the original array, will access its elements in their sorted order. It does not modify the original array.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsanarrayofindiceswhichwhenusedtoindextheoriginalarraywillaccessitselementsintheirsortedorder.Itdoesnotmodifytheoriginalarray.)

Returns an array of indices which, when used to index the original array, will access its elements in their sorted order. It does not modify the original array.
Syntax & Overloads
array.sort_indices(id, order) → array<int>
array.sort_indices(id, order, sort_field) → array<int>
Arguments
id (array<int/float/string>) An array object.
order (series sort_order) The sort order: order.ascending or order.descending. Optional. The default is order.ascending.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.sort_indices")
a = array.from(5, -2, 0, 9, 1)
sortedIndices = array.sort_indices(a) // [1, 2, 4, 0, 3]
indexOfSmallestValue = array.get(sortedIndices, 0) // 1
smallestValue = array.get(a, indexOfSmallestValue) // -2
plot(smallestValue)
See also
array.new_float()
array.insert()
array.slice()
array.reverse()
order.ascending
order.descending
array.standardize()2 overloads

## [The function returns the array of standardized elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthearrayofstandardizedelements.)

The function returns the array of standardized elements.
Syntax & Overloads
array.standardize(id) → array<float>
array.standardize(id) → array<int>
Arguments
id (array<int/float>) An array object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.standardize example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
b = array.standardize(a)
plot(array.min(b))
plot(array.max(b))
Returns
The array of standardized elements.
See also
array.max()
array.min()
array.mode()
array.avg()
array.variance()
array.stdev()
array.stdev()2 overloads

## [The function returns the standard deviation of an array's elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthestandarddeviationofanarrayselements.)

The function returns the standard deviation of an array's elements.
Syntax & Overloads
array.stdev(id, biased) → series float
array.stdev(id, biased) → series int
Arguments
id (array<int/float>) An array object.
biased (series bool) Determines which estimate should be used. Optional. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.stdev example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.stdev(a))
Returns
The standard deviation of the array's elements.
Remarks
If biased is true, the function calculates using a biased estimate of the entire population. If biased is false, it uses an unbiased estimate of a sample.
Returns na if the id array is empty.
See also
array.new_float()
array.max()
array.min()
array.avg()
array.sum()2 overloads

## [The function returns the sum of an array's elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthesumofanarrayselements.)

The function returns the sum of an array's elements.
Syntax & Overloads
array.sum(id) → series float
array.sum(id) → series int
Arguments
id (array<int/float>) An array object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.sum example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.sum(a))
Returns
The sum of the array's elements.
Remarks
Returns na if the id array is empty.
See also
array.new_float()
array.max()
array.min()
array.unshift()

## [The function inserts the value at the beginning of the array.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioninsertsthevalueatthebeginningofthearray.)

The function inserts the value at the beginning of the array.
Syntax
array.unshift(id, value) → void
Arguments
id (any array type) An array object.
value (series <type of the array's elements>) The value to add to the start of the array.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.unshift example")
a = array.new_float(5, 0)
array.unshift(a, open)
plot(array.get(a, 0))
See also
array.shift()
array.set()
array.insert()
array.remove()
array.indexof()
array.variance()2 overloads

## [The function returns the variance of an array's elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthevarianceofanarrayselements.)

The function returns the variance of an array's elements.
Syntax & Overloads
array.variance(id, biased) → series float
array.variance(id, biased) → series int
Arguments
id (array<int/float>) An array object.
biased (series bool) Determines which estimate should be used. Optional. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("array.variance example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.variance(a))
Returns
The variance of the array's elements.
Remarks
If biased is true, function will calculate using a biased estimate of the entire population, if false - unbiased estimate of a sample.
Returns na if the id array is empty.
See also
array.new_float()
array.stdev()
array.min()
array.avg()
array.covariance()
barcolor()

## [Set color of bars.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setcolorofbars.)

Set color of bars.
Syntax
barcolor(color, offset, editable, show_last, title, display) → void
Arguments
color (series color) Color of bars. You can use constants like 'red' or '#ff001a' as well as complex expressions like 'close >= open ? color.green : color.red'. Required argument.
offset (simple int) Shifts the color series to the left or to the right on the given number of bars. Default is 0.
editable (input bool) If true then barcolor style will be editable in Format dialog. Default is true.
show_last (input int) Optional. The number of bars, counting backwards from the most recent bar, on which the function can draw.
title (const string) Title of the barcolor. Optional argument.
display (input plot_simple_display) Controls where the barcolor is displayed. Possible values are: display.none, display.all. Default is display.all.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("barcolor example", overlay=true)
barcolor(close < open ? color.black : color.white)
See also
bgcolor()
plot()
fill()
bgcolor()

## [Fill background of bars with specified color.](https://www.tradingview.com/pine-script-reference/v6/#fun_Fillbackgroundofbarswithspecifiedcolor.)

Fill background of bars with specified color.
Syntax
bgcolor(color, offset, editable, show_last, title, display, force_overlay) → void
Arguments
color (series color) Color of the filled background. You can use constants like 'red' or '#ff001a' as well as complex expressions like 'close >= open ? color.green : color.red'. Required argument.
offset (simple int) Shifts the color series to the left or to the right on the given number of bars. Default is 0.
editable (input bool) If true then bgcolor style will be editable in Format dialog. Default is true.
show_last (input int) Optional. The number of bars, counting backwards from the most recent bar, on which the function can draw.
title (const string) Title of the bgcolor. Optional argument.
display (input plot_simple_display) Controls where the bgcolor is displayed. Possible values are: display.none, display.all. Default is display.all.
force_overlay (const bool) If true, the plotted results will display on the main chart pane, even when the script occupies a separate pane. Optional. The default is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("bgcolor example", overlay=true)
bgcolor(close < open ? color.new(color.red,70) : color.new(color.green, 70))
See also
barcolor()
plot()
fill()
bool()4 overloads

## [Converts the x value to a bool value. Returns false if x is na, false, or an int/float value equal to 0. Returns true for all other possible values.](https://www.tradingview.com/pine-script-reference/v6/#fun_Convertsthexvaluetoaboolvalue.Returnsfalseifxisnafalseoranintfloatvalueequalto0.Returnstrueforallotherpossiblevalues.)

Converts the x value to a bool value. Returns false if x is na, false, or an int/float value equal to 0. Returns true for all other possible values.
Syntax & Overloads
bool(x) → const bool
bool(x) → input bool
bool(x) → simple bool
bool(x) → series bool
Arguments
x (simple int/float/bool) The value to convert to the specified type, usually na.
Returns
The value of the argument after casting to bool.
See also
float()
int()
color()
string()
line()
label()
box()

## [Casts na to box.](https://www.tradingview.com/pine-script-reference/v6/#fun_Castsnatobox.)

Casts na to box.
Syntax
box(x) → series box
Arguments
x (series box) The value to convert to the specified type, usually na.
Returns
The value of the argument after casting to box.
See also
float()
int()
bool()
color()
string()
line()
label()
box.copy()

## [Clones the box object.](https://www.tradingview.com/pine-script-reference/v6/#fun_Clonestheboxobject.)

Clones the box object.
Syntax
box.copy(id) → series box
Arguments
id (series box) Box object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator('Last 50 bars price ranges', overlay = true)
LOOKBACK = 50
highest = ta.highest(LOOKBACK)
lowest = ta.lowest(LOOKBACK)
if barstate.islastconfirmedhistory
    var BoxLast = box.new(bar_index[LOOKBACK], highest, bar_index, lowest, bgcolor = color.new(color.green, 80))
    var BoxPrev = box.copy(BoxLast)
    box.set_lefttop(BoxPrev, bar_index[LOOKBACK * 2], highest[50])
    box.set_rightbottom(BoxPrev, bar_index[LOOKBACK], lowest[50])
    box.set_bgcolor(BoxPrev, color.new(color.red, 80))
See also
box.new()
box.delete()
box.delete()

## [Deletes the specified box object. If it has already been deleted, does nothing.](https://www.tradingview.com/pine-script-reference/v6/#fun_Deletesthespecifiedboxobject.Ifithasalreadybeendeleteddoesnothing.)

Deletes the specified box object. If it has already been deleted, does nothing.
Syntax
box.delete(id) → void
Arguments
id (series box) A box object to delete.
See also
box.new()
box.get_bottom()

## [Returns the price value of the bottom border of the box.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthepricevalueofthebottomborderofthebox.)

Returns the price value of the bottom border of the box.
Syntax
box.get_bottom(id) → series float
Arguments
id (series box) A box object.
Returns
The price value.
See also
box.new()
box.set_bottom()
box.get_left()

## [Returns the bar index or the UNIX time (depending on the last value used for 'xloc') of the left border of the box.](https://www.tradingview.com/pine-script-reference/v6/#fun_ReturnsthebarindexortheUNIXtimedependingonthelastvalueusedforxlocoftheleftborderofthebox.)

Returns the bar index or the UNIX time (depending on the last value used for 'xloc') of the left border of the box.
Syntax
box.get_left(id) → series int
Arguments
id (series box) A box object.
Returns
A bar index or a UNIX timestamp (in milliseconds).
See also
box.new()
box.set_left()
box.get_right()

## [Returns the bar index or the UNIX time (depending on the last value used for 'xloc') of the right border of the box.](https://www.tradingview.com/pine-script-reference/v6/#fun_ReturnsthebarindexortheUNIXtimedependingonthelastvalueusedforxlocoftherightborderofthebox.)

Returns the bar index or the UNIX time (depending on the last value used for 'xloc') of the right border of the box.
Syntax
box.get_right(id) → series int
Arguments
id (series box) A box object.
Returns
A bar index or a UNIX timestamp (in milliseconds).
See also
box.new()
box.set_right()
box.get_top()

## [Returns the price value of the top border of the box.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthepricevalueofthetopborderofthebox.)

Returns the price value of the top border of the box.
Syntax
box.get_top(id) → series float
Arguments
id (series box) A box object.
Returns
The price value.
See also
box.new()
box.set_top()
box.new()2 overloads

## [Creates a new box object.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createsanewboxobject.)

Creates a new box object.
Syntax & Overloads
box.new(top_left, bottom_right, border_color, border_width, border_style, extend, xloc, bgcolor, text, text_size, text_color, text_halign, text_valign, text_wrap, text_font_family, force_overlay, text_formatting) → series box
box.new(left, top, right, bottom, border_color, border_width, border_style, extend, xloc, bgcolor, text, text_size, text_color, text_halign, text_valign, text_wrap, text_font_family, force_overlay, text_formatting) → series box
Arguments
top_left (chart.point) A chart.point object that specifies the top-left corner location of the box.
bottom_right (chart.point) A chart.point object that specifies the bottom-right corner location of the box.
border_color (series color) Color of the four borders. Optional. The default is color.blue.
border_width (series int) Width of the four borders, in pixels. Optional. The default is 1 pixel.
border_style (series string) Style of the four borders. Possible values: line.style_solid, line.style_dotted, line.style_dashed. Optional. The default value is line.style_solid.
extend (series string) When extend.none is used, the horizontal borders start at the left border and end at the right border. With extend.left or extend.right, the horizontal borders are extended indefinitely to the left or right of the box, respectively. With extend.both, the horizontal borders are extended on both sides. Optional. The default value is extend.none.
xloc (series string) Determines whether the arguments to 'left' and 'right' are a bar index or a time value. If xloc = xloc.bar_index, the arguments must be a bar index. If xloc = xloc.bar_time, the arguments must be a UNIX time. Possible values: xloc.bar_index and xloc.bar_time. Optional. The default is xloc.bar_index.
bgcolor (series color) Background color of the box. Optional. The default is color.blue.
text (series string) The text to be displayed inside the box. Optional. The default is empty string.
text_size (series int/string) Optional. Size of the box's text. The size can be any positive integer, or one of the size.* built-in constant strings. The constant strings and their equivalent integer values are: size.auto (0), size.tiny (8), size.small (10), size.normal (14), size.large (20), size.huge (36). The default value is size.auto or 0.
text_color (series color) The color of the text. Optional. The default is color.black.
text_halign (series string) The horizontal alignment of the box's text. Optional. The default value is text.align_center. Possible values: text.align_left, text.align_center, text.align_right.
text_valign (series string) The vertical alignment of the box's text. Optional. The default value is text.align_center. Possible values: text.align_top, text.align_center, text.align_bottom.
text_wrap (series string) Optional. Whether to wrap text. Wrapped text starts a new line when it reaches the side of the box. Wrapped text lower than the bottom of the box is not displayed. Unwrapped text stays on a single line and is displayed past the width of the box if it is too long. If the text_size is 0 or text.wrap_auto, this setting has no effect. The default value is text.wrap_none. Possible values: text.wrap_none, text.wrap_auto.
text_font_family (series string) The font family of the text. Optional. The default value is font.family_default. Possible values: font.family_default, font.family_monospace.
force_overlay (const bool) If true, the drawing will display on the main chart pane, even when the script occupies a separate pane. Optional. The default is false.
text_formatting (series text_format) The formatting of the displayed text. Formatting options support addition. For example, text.format_bold + text.format_italic will make the text both bold and italicized. Possible values: text.format_none, text.format_bold, text.format_italic. Optional. The default is text.format_none.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("box.new")
var b = box.new(time, open, time + 60 *60* 24, close, xloc=xloc.bar_time, border_style=line.style_dashed)
box.set_lefttop(b, time, 100)
box.set_rightbottom(b, time + 60 *60* 24, 500)
box.set_bgcolor(b, color.green)
Returns
The ID of a box object which may be used in box.set_*() and box.get_*() functions.
See also
box.delete()
box.get_left()
box.get_top()
box.get_right()
box.get_bottom()
box.set_top_left_point()
box.set_left()
box.set_top()
box.set_bottom_right_point()
box.set_right()
box.set_bottom()
box.set_border_color()
box.set_bgcolor()
box.set_border_width()
box.set_border_style()
box.set_extend()
box.set_text()
box.set_text_formatting()
box.set_xloc()
box.set_bgcolor()

## [Sets the background color of the box.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsthebackgroundcolorofthebox.)

Sets the background color of the box.
Syntax
box.set_bgcolor(id, color) → void
Arguments
id (series box) A box object.
color (series color) New background color.
See also
box.new()
box.set_border_color()

## [Sets the border color of the box.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsthebordercolorofthebox.)

Sets the border color of the box.
Syntax
box.set_border_color(id, color) → void
Arguments
id (series box) A box object.
color (series color) New border color.
See also
box.new()
box.set_border_style()

## [Sets the border style of the box.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setstheborderstyleofthebox.)

Sets the border style of the box.
Syntax
box.set_border_style(id, style) → void
Arguments
id (series box) A box object.
style (series string) New border style.
See also
box.new()
line.style_solid
line.style_dotted
line.style_dashed
box.set_border_width()

## [Sets the border width of the box.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setstheborderwidthofthebox.)

Sets the border width of the box.
Syntax
box.set_border_width(id, width) → void
Arguments
id (series box) A box object.
width (series int) Width of the four borders, in pixels.
See also
box.new()
box.set_bottom()

## [Sets the bottom coordinate of the box.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsthebottomcoordinateofthebox.)

Sets the bottom coordinate of the box.
Syntax
box.set_bottom(id, bottom) → void
Arguments
id (series box) A box object.
bottom (series int/float) Price value of the bottom border.
See also
box.new()
box.get_bottom()
box.set_bottom_right_point()

## [Sets the bottom-right corner location of the id box to point.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsthebottom-rightcornerlocationoftheidboxtopoint.)

Sets the bottom-right corner location of the id box to point.
Syntax
box.set_bottom_right_point(id, point) → void
Arguments
id (series box) A box object.
point (chart.point) A chart.point object.
box.set_extend()

## [Sets extending type of the border of this box object. When extend.none is used, the horizontal borders start at the left border and end at the right border. With extend.left or extend.right, the horizontal borders are extended indefinitely to the left or right of the box, respectively. With extend.both, the horizontal borders are extended on both sides.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsextendingtypeoftheborderofthisboxobject.Whenextend.noneisusedthehorizontalbordersstartattheleftborderandendattherightborder.Withextend.leftorextend.rightthehorizontalbordersareextendedindefinitelytotheleftorrightoftheboxrespectively.Withextend.boththehorizontalbordersareextendedonbothsides.)

Sets extending type of the border of this box object. When extend.none is used, the horizontal borders start at the left border and end at the right border. With extend.left or extend.right, the horizontal borders are extended indefinitely to the left or right of the box, respectively. With extend.both, the horizontal borders are extended on both sides.
Syntax
box.set_extend(id, extend) → void
Arguments
id (series box) A box object.
extend (series string) New extending type.
See also
box.new()
extend.none
extend.right
extend.left
extend.both
box.set_left()

## [Sets the left coordinate of the box.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setstheleftcoordinateofthebox.)

Sets the left coordinate of the box.
Syntax
box.set_left(id, left) → void
Arguments
id (series box) A box object.
left (series int) Bar index or bar time of the left border. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
See also
box.new()
box.get_left()
box.set_lefttop()

## [Sets the left and top coordinates of the box.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setstheleftandtopcoordinatesofthebox.)

Sets the left and top coordinates of the box.
Syntax
box.set_lefttop(id, left, top) → void
Arguments
id (series box) A box object.
left (series int) Bar index or bar time of the left border.
top (series int/float) Price value of the top border.
See also
box.new()
box.get_left()
box.get_top()
box.set_right()

## [Sets the right coordinate of the box.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setstherightcoordinateofthebox.)

Sets the right coordinate of the box.
Syntax
box.set_right(id, right) → void
Arguments
id (series box) A box object.
right (series int) Bar index or bar time of the right border. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
See also
box.new()
box.get_right()
box.set_rightbottom()

## [Sets the right and bottom coordinates of the box.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setstherightandbottomcoordinatesofthebox.)

Sets the right and bottom coordinates of the box.
Syntax
box.set_rightbottom(id, right, bottom) → void
Arguments
id (series box) A box object.
right (series int) Bar index or bar time of the right border.
bottom (series int/float) Price value of the bottom border.
See also
box.new()
box.get_right()
box.get_bottom()
box.set_text()

## [The function sets the text in the box.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionsetsthetextinthebox.)

The function sets the text in the box.
Syntax
box.set_text(id, text) → void
Arguments
id (series box) A box object.
text (series string) The text to be displayed inside the box.
See also
box.set_text_color()
box.set_text_size()
box.set_text_valign()
box.set_text_halign()
box.set_text_formatting()
box.set_text_color()

## [The function sets the color of the text inside the box.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionsetsthecolorofthetextinsidethebox.)

The function sets the color of the text inside the box.
Syntax
box.set_text_color(id, text_color) → void
Arguments
id (series box) A box object.
text_color (series color) The color of the text.
See also
box.set_text()
box.set_text_size()
box.set_text_valign()
box.set_text_halign()
box.set_text_font_family()

## [The function sets the font family of the text inside the box.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionsetsthefontfamilyofthetextinsidethebox.)

The function sets the font family of the text inside the box.
Syntax
box.set_text_font_family(id, text_font_family) → void
Arguments
id (series box) A box object.
text_font_family (series string) The font family of the text. Possible values: font.family_default, font.family_monospace.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("Example of setting the box font")
if barstate.islastconfirmedhistory
    b = box.new(bar_index, open-ta.tr, bar_index-50, open-ta.tr*5, text="monospace")
    box.set_text_font_family(b, font.family_monospace)
See also
box.new()
font.family_default
font.family_monospace
box.set_text_formatting()

## [Sets the formatting attributes the drawing applies to displayed text.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setstheformattingattributesthedrawingappliestodisplayedtext.)

Sets the formatting attributes the drawing applies to displayed text.
Syntax
box.set_text_formatting(id, text_formatting) → void
Arguments
id (series box) A box object.
text_formatting (series text_format) The formatting of the displayed text. Formatting options support addition. For example, text.format_bold + text.format_italic will make the text both bold and italicized. Possible values: text.format_none, text.format_bold, text.format_italic. Optional. The default is text.format_none.
See also
box.set_text_color()
box.set_text_size()
box.set_text_valign()
box.set_text_halign()
box.set_text()
box.set_text_halign()

## [The function sets the horizontal alignment of the box's text.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionsetsthehorizontalalignmentoftheboxstext.)

The function sets the horizontal alignment of the box's text.
Syntax
box.set_text_halign(id, text_halign) → void
Arguments
id (series box) A box object.
text_halign (series string) The horizontal alignment of a box's text. Possible values: text.align_left, text.align_center, text.align_right.
See also
box.set_text()
box.set_text_size()
box.set_text_valign()
box.set_text_color()
box.set_text_size()

## [The function sets the size of the box's text.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionsetsthesizeoftheboxstext.)

The function sets the size of the box's text.
Syntax
box.set_text_size(id, text_size) → void
Arguments
id (series box) A box object.
text_size (series int/string) Size of the box's text. The size can be any positive integer, or one of the size.* built-in constant strings. The constant strings and their equivalent integer values are: size.auto (0), size.tiny (8), size.small (10), size.normal (14), size.large (20), size.huge (36).
See also
box.set_text()
box.set_text_color()
box.set_text_valign()
box.set_text_halign()
box.set_text_valign()

## [The function sets the vertical alignment of a box's text.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionsetstheverticalalignmentofaboxstext.)

The function sets the vertical alignment of a box's text.
Syntax
box.set_text_valign(id, text_valign) → void
Arguments
id (series box) A box object.
text_valign (series string) The vertical alignment of the box's text. Possible values: text.align_top, text.align_center, text.align_bottom.
See also
box.set_text()
box.set_text_size()
box.set_text_color()
box.set_text_halign()
box.set_text_wrap()

## [The function sets the mode of wrapping of the text inside the box.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionsetsthemodeofwrappingofthetextinsidethebox.)

The function sets the mode of wrapping of the text inside the box.
Syntax
box.set_text_wrap(id, text_wrap) → void
Arguments
id (series box) A box object.
text_wrap (series string) Whether to wrap text. Wrapped text starts a new line when it reaches the side of the box. Wrapped text lower than the bottom of the box is not displayed. Unwrapped text stays on a single line and is displayed past the width of the box if it is too long. If the text_size is 0 or text.wrap_auto, this setting has no effect. Possible values: text.wrap_none, text.wrap_auto.
See also
box.set_text()
box.set_text_size()
box.set_text_valign()
box.set_text_halign()
box.set_text_color()
box.set_top()

## [Sets the top coordinate of the box.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsthetopcoordinateofthebox.)

Sets the top coordinate of the box.
Syntax
box.set_top(id, top) → void
Arguments
id (series box) A box object.
top (series int/float) Price value of the top border.
See also
box.new()
box.get_top()
box.set_top_left_point()

## [Sets the top-left corner location of the id box to point.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsthetop-leftcornerlocationoftheidboxtopoint.)

Sets the top-left corner location of the id box to point.
Syntax
box.set_top_left_point(id, point) → void
Arguments
id (series box) A box object.
point (chart.point) A chart.point object.
box.set_xloc()

## [Sets the left and right borders of a box and updates its xloc property.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setstheleftandrightbordersofaboxandupdatesitsxlocproperty.)

Sets the left and right borders of a box and updates its xloc property.
Syntax
box.set_xloc(id, left, right, xloc) → void
Arguments
id (series box) The ID of the box object to update.
left (series int) The bar index or timestamp for the left border of the box.
right (series int) The bar index or timestamp for the right border of the box.
xloc (series string) Determines whether the box treats the left and right arguments as bar indices or timestamps. Possible values: xloc.bar_index and xloc.bar_time. If the value is xloc.bar_index, the arguments represent bar indices. If xloc.bar_time, the arguments represent UNIX timestamps.
See also
box.new()
xloc.bar_index
xloc.bar_time
chart.point.copy()

## [Creates a copy of a chart.point object with the specified id.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createsacopyofachart.pointobjectwiththespecifiedid.)

Creates a copy of a chart.point object with the specified id.
Syntax
chart.point.copy(id) → chart.point
Arguments
id (chart.point) A chart.point object.
chart.point.from_index()

## [Returns a chart.point object with index as its x-coordinate and price as its y-coordinate.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsachart.pointobjectwithindexasitsx-coordinateandpriceasitsy-coordinate.)

Returns a chart.point object with index as its x-coordinate and price as its y-coordinate.
Syntax
chart.point.from_index(index, price) → chart.point
Arguments
index (series int) The x-coordinate of the point, expressed as a bar index value.
price (series int/float) The y-coordinate of the point.
Remarks
The time field values of chart.point instances returned from this function will be na, meaning drawing objects with xloc values set to xloc.bar_time will not work with them.
chart.point.from_time()

## [Returns a chart.point object with time as its x-coordinate and price as its y-coordinate.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsachart.pointobjectwithtimeasitsx-coordinateandpriceasitsy-coordinate.)

Returns a chart.point object with time as its x-coordinate and price as its y-coordinate.
Syntax
chart.point.from_time(time, price) → chart.point
Arguments
time (series int) The x-coordinate of the point, expressed as a UNIX time value, in milliseconds.
price (series int/float) The y-coordinate of the point.
Remarks
The index field values of chart.point instances returned from this function will be na, meaning drawing objects with xloc values set to xloc.bar_index will not work with them.
chart.point.new()

## [Creates a new chart.point object with the specified time, index, and price.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createsanewchart.pointobjectwiththespecifiedtimeindexandprice.)

Creates a new chart.point object with the specified time, index, and price.
Syntax
chart.point.new(time, index, price) → chart.point
Arguments
time (series int) The x-coordinate of the point, expressed as a UNIX time value, in milliseconds.
index (series int) The x-coordinate of the point, expressed as a bar index value.
price (series int/float) The y-coordinate of the point.
Remarks
Whether a drawing object uses a point's time or index field as an x-coordinate depends on the xloc type used in the function call that returned the drawing.
It's important to note that this function does not verify that the time and index values refer to the same bar.
See also
polyline.new()
chart.point.now()

## [Returns a chart.point object with price as the y-coordinate](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsachart.pointobjectwithpriceasthey-coordinate)

Returns a chart.point object with price as the y-coordinate
Syntax
chart.point.now(price) → chart.point
Arguments
price (series int/float) The y-coordinate of the point. Optional. The default is close.
Remarks
The chart.point instance returned from this function records values for its index and time fields on the bar it executed on, making it suitable for use with drawing objects of any xloc type.
color()4 overloads

## [Casts na to color](https://www.tradingview.com/pine-script-reference/v6/#fun_Castsnatocolor)

Casts na to color
Syntax & Overloads
color(x) → const color
color(x) → input color
color(x) → simple color
color(x) → series color
Arguments
x (const color) The value to convert to the specified type, usually na.
Returns
The value of the argument after casting to color.
See also
float()
int()
bool()
string()
line()
label()
color.b()4 overloads

## [Retrieves the value of the color's blue component.](https://www.tradingview.com/pine-script-reference/v6/#fun_Retrievesthevalueofthecolorsbluecomponent.)

Retrieves the value of the color's blue component.
Syntax & Overloads
color.b(color) → const float
color.b(color) → input float
color.b(color) → simple float
color.b(color) → series float
Arguments
color (const color) Color.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("color.b", overlay=true)
plot(color.b(color.blue))
Returns
The value (0 to 255) of the color's blue component.
color.from_gradient()

## [Based on the relative position of value in the bottom_value to top_value range, the function returns a color from the gradient defined by bottom_color to top_color.](https://www.tradingview.com/pine-script-reference/v6/#fun_Basedontherelativepositionofvalueinthebottom_valuetotop_valuerangethefunctionreturnsacolorfromthegradientdefinedbybottom_colortotop_color.)

Based on the relative position of value in the bottom_value to top_value range, the function returns a color from the gradient defined by bottom_color to top_color.
Syntax
color.from_gradient(value, bottom_value, top_value, bottom_color, top_color) → series color
Arguments
value (series int/float) Value to calculate the position-dependent color.
bottom_value (series int/float) Bottom position value corresponding to bottom_color.
top_value (series int/float) Top position value corresponding to top_color.
bottom_color (series color) Bottom position color.
top_color (series color) Top position color.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("color.from_gradient", overlay=true)
color1 = color.from_gradient(close, low, high, color.yellow, color.lime)
color2 = color.from_gradient(ta.rsi(close, 7), 0, 100, color.rgb(255, 0, 0), color.rgb(0, 255, 0, 50))
plot(close, color=color1)
plot(ta.rsi(close,7), color=color2)
Returns
A color calculated from the linear gradient between bottom_color to top_color.
Remarks
Using this function will have an impact on the colors displayed in the script's "Settings/Style" tab. See the User Manual for more information.
color.g()4 overloads

## [Retrieves the value of the color's green component.](https://www.tradingview.com/pine-script-reference/v6/#fun_Retrievesthevalueofthecolorsgreencomponent.)

Retrieves the value of the color's green component.
Syntax & Overloads
color.g(color) → const float
color.g(color) → input float
color.g(color) → simple float
color.g(color) → series float
Arguments
color (const color) Color.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("color.g", overlay=true)
plot(color.g(color.green))
Returns
The value (0 to 255) of the color's green component.
color.new()4 overloads

## [Function color applies the specified transparency to the given color.](https://www.tradingview.com/pine-script-reference/v6/#fun_Functioncolorappliesthespecifiedtransparencytothegivencolor.)

Function color applies the specified transparency to the given color.
Syntax & Overloads
color.new(color, transp) → const color
color.new(color, transp) → input color
color.new(color, transp) → simple color
color.new(color, transp) → series color
Arguments
color (const color) Color to apply transparency to.
transp (const int/float) Possible values are from 0 (not transparent) to 100 (invisible).
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("color.new", overlay=true)
plot(close, color=color.new(color.red, 50))
Returns
Color with specified transparency.
Remarks
Using arguments that are not constants (e.g., 'simple', 'input' or 'series') will have an impact on the colors displayed in the script's "Settings/Style" tab. See the User Manual for more information.
color.r()4 overloads

## [Retrieves the value of the color's red component.](https://www.tradingview.com/pine-script-reference/v6/#fun_Retrievesthevalueofthecolorsredcomponent.)

Retrieves the value of the color's red component.
Syntax & Overloads
color.r(color) → const float
color.r(color) → input float
color.r(color) → simple float
color.r(color) → series float
Arguments
color (const color) Color.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("color.r", overlay=true)
plot(color.r(color.red))
Returns
The value (0 to 255) of the color's red component.
color.rgb()4 overloads

## [Creates a new color with transparency using the RGB color model.](https://www.tradingview.com/pine-script-reference/v6/#fun_CreatesanewcolorwithtransparencyusingtheRGBcolormodel.)

Creates a new color with transparency using the RGB color model.
Syntax & Overloads
color.rgb(red, green, blue, transp) → const color
color.rgb(red, green, blue, transp) → input color
color.rgb(red, green, blue, transp) → simple color
color.rgb(red, green, blue, transp) → series color
Arguments
red (const int/float) Red color component. Possible values are from 0 to 255.
green (const int/float) Green color component. Possible values are from 0 to 255.
blue (const int/float) Blue color component. Possible values are from 0 to 255.
transp (const int/float) Optional. Color transparency. Possible values are from 0 (opaque) to 100 (invisible). Default value is 0.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("color.rgb", overlay=true)
plot(close, color=color.rgb(255, 0, 0, 50))
Returns
Color with specified transparency.
Remarks
Using arguments that are not constants (e.g., 'simple', 'input' or 'series') will have an impact on the colors displayed in the script's "Settings/Style" tab. See the User Manual for more information.
color.t()4 overloads

## [Retrieves the color's transparency.](https://www.tradingview.com/pine-script-reference/v6/#fun_Retrievesthecolorstransparency.)

Retrieves the color's transparency.
Syntax & Overloads
color.t(color) → const float
color.t(color) → input float
color.t(color) → simple float
color.t(color) → series float
Arguments
color (const color) Color.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("color.t", overlay=true)
plot(color.t(color.new(color.red, 50)))
Returns
The value (0-100) of the color's transparency.
dayofmonth()

## [Calculates the day number of the month, in a specified time zone, from a UNIX timestamp.](https://www.tradingview.com/pine-script-reference/v6/#fun_CalculatesthedaynumberofthemonthinaspecifiedtimezonefromaUNIXtimestamp.)

Calculates the day number of the month, in a specified time zone, from a UNIX timestamp.
Syntax
dayofmonth(time, timezone) → series int
Arguments
time (series int) A UNIX timestamp in milliseconds.
timezone (series string) Optional. Specifies the time zone of the returned day number. The value can be a time zone string in UTC/GMT offset notation (e.g., "UTC-5") or IANA time zone database notation (e.g., "America/New_York"). The default is syminfo.timezone.
Returns
The calculated day of the month, expressed in the specified time zone.
Remarks
A UNIX timestamp represents the number of milliseconds elapsed since 00:00 UTC on 1970-01-01. The meaning of a UNIX timestamp does not change relative to any time zone.
See also
dayofmonth
dayofweek()
weekofyear()
time()
year()
month()
hour()
minute()
second()
dayofweek()

## [Calculates the day number of the week, in a specified time zone, from a UNIX timestamp.](https://www.tradingview.com/pine-script-reference/v6/#fun_CalculatesthedaynumberoftheweekinaspecifiedtimezonefromaUNIXtimestamp.)

Calculates the day number of the week, in a specified time zone, from a UNIX timestamp.
Syntax
dayofweek(time, timezone) → series int
Arguments
time (series int) A UNIX timestamp in milliseconds.
timezone (series string) Optional. Specifies the time zone of the returned day number. The value can be a time zone string in UTC/GMT offset notation (e.g., "UTC-5") or IANA time zone database notation (e.g., "America/New_York"). The default is syminfo.timezone.
Returns
The calculated day number, expressed in the specified time zone.
Remarks
A UNIX timestamp represents the number of milliseconds elapsed since 00:00 UTC on 1970-01-01. The meaning of a UNIX timestamp does not change relative to any time zone.
See also
dayofweek
dayofmonth()
weekofyear()
time()
year()
month()
hour()
minute()
second()
fill()3 overloads

## [Fills background between two plots or hlines with a given color.](https://www.tradingview.com/pine-script-reference/v6/#fun_Fillsbackgroundbetweentwoplotsorhlineswithagivencolor.)

Fills background between two plots or hlines with a given color.
Syntax & Overloads
fill(hline1, hline2, color, title, editable, fillgaps, display) → void
fill(plot1, plot2, color, title, editable, show_last, fillgaps, display) → void
fill(plot1, plot2, top_value, bottom_value, top_color, bottom_color, title, display, fillgaps, editable) → void
Arguments
hline1 (hline) The first hline object. Required argument.
hline2 (hline) The second hline object. Required argument.
color (series color) Color of the background fill. You can use constants like 'color=color.red' or 'color=#ff001a' as well as complex expressions like 'color = close >= open ? color.green : color.red'. Optional argument.
title (const string) Title of the created fill object. Optional argument.
editable (input bool) If true then fill style will be editable in Format dialog. Default is true.
fillgaps (const bool) Controls continuing fills on gaps, i.e., when one of the plot() calls returns an na value. When true, the last fill will continue on gaps. The default is false.
display (input plot_simple_display) Controls where the fill is displayed. Possible values are: display.none, display.all. Default is display.all.
Fill between two horizontal lines
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("Fill between hlines", overlay = false)
h1 = hline(20)
h2 = hline(10)
fill(h1, h2, color = color.new(color.blue, 90))
Fill between two plots
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("Fill between plots", overlay = true)
p1 = plot(open)
p2 = plot(close)
fill(p1, p2, color = color.new(color.green, 90))
Gradient fill between two horizontal lines
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("Gradient Fill between hlines", overlay = false)
topVal = input.int(100)
botVal = input.int(0)
topCol = input.color(color.red)
botCol = input.color(color.blue)
topLine = hline(100, color = topCol, linestyle = hline.style_solid)
botLine = hline(0,   color = botCol, linestyle = hline.style_solid)
fill(topLine, botLine, topVal, botVal, topCol, botCol)
See also
plot()
barcolor()
bgcolor()
hline()
color.new()
fixnan()3 overloads

## [For a given series replaces NaN values with previous nearest non-NaN value.](https://www.tradingview.com/pine-script-reference/v6/#fun_ForagivenseriesreplacesNaNvalueswithpreviousnearestnon-NaNvalue.)

For a given series replaces NaN values with previous nearest non-NaN value.
Syntax & Overloads
fixnan(source) → series color
fixnan(source) → series int
fixnan(source) → series float
Arguments
source (series color) Source used for the calculation.
Returns
Series without na gaps.
See also
na()
na
nz()
float()4 overloads

## [Casts na to float](https://www.tradingview.com/pine-script-reference/v6/#fun_Castsnatofloat)

Casts na to float
Syntax & Overloads
float(x) → const float
float(x) → input float
float(x) → simple float
float(x) → series float
Arguments
x (const int/float) The value to convert to the specified type, usually na.
Returns
The value of the argument after casting to float.
See also
int()
bool()
color()
string()
line()
label()
footprint.buy_volume()

## [Calculates the total "buy" volume for the volume footprint represented by a footprint object.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculatesthetotalbuyvolumeforthevolumefootprintrepresentedbyafootprintobject.)

Calculates the total "buy" volume for the volume footprint represented by a footprint object.
Syntax
footprint.buy_volume(id) → series float
Arguments
id (footprint) The reference (ID) of the footprint object to analyze.
Returns
The total "buy" volume measured by the footprint.
footprint.delta()

## [Calculates the overall volume delta for the volume footprint represented by a footprint object. The value represents the difference between the footprint's total "buy" volume and "sell" volume. A positive value indicates that the total "buy" volume in the footprint exceeds the total "sell" volume, and a negative value indicates the opposite.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculatestheoverallvolumedeltaforthevolumefootprintrepresentedbyafootprintobject.Thevaluerepresentsthedifferencebetweenthefootprintstotalbuyvolumeandsellvolume.Apositivevalueindicatesthatthetotalbuyvolumeinthefootprintexceedsthetotalsellvolumeandanegativevalueindicatestheopposite.)

Calculates the overall volume delta for the volume footprint represented by a footprint object. The value represents the difference between the footprint's total "buy" volume and "sell" volume. A positive value indicates that the total "buy" volume in the footprint exceeds the total "sell" volume, and a negative value indicates the opposite.
Syntax
footprint.delta(id) → series float
Arguments
id (footprint) The reference (ID) of the footprint object to analyze.
Returns
The overall volume delta for the footprint.
footprint.get_row_by_price()

## [Analyzes the volume footprint represented by a footprint object to find the row whose price range includes the specified price level. If the price belongs to one of the rows, the function returns the ID of the volume_row object that contains the data for that row. Otherwise, it returns na.](https://www.tradingview.com/pine-script-reference/v6/#fun_Analyzesthevolumefootprintrepresentedbyafootprintobjecttofindtherowwhosepricerangeincludesthespecifiedpricelevel.IfthepricebelongstooneoftherowsthefunctionreturnstheIDofthevolume_rowobjectthatcontainsthedataforthatrow.Otherwiseitreturnsna.)

Analyzes the volume footprint represented by a footprint object to find the row whose price range includes the specified price level. If the price belongs to one of the rows, the function returns the ID of the volume_row object that contains the data for that row. Otherwise, it returns na.
Syntax
footprint.get_row_by_price(id, price) → volume_row
Arguments
id (footprint) The reference (ID) of the footprint object to analyze.
price (series int/float) The price value for which to find the corresponding footprint row.
Returns
The ID of a volume_row object representing the footprint row that contains the specified price, or na if the price is outside the footprint's price range.
footprint.poc()

## [Finds the Point of Control (POC) row for the volume footprint represented by a footprint object, then returns the ID of a volume_row object containing the data for that row.](https://www.tradingview.com/pine-script-reference/v6/#fun_FindsthePointofControlPOCrowforthevolumefootprintrepresentedbyafootprintobjectthenreturnstheIDofavolume_rowobjectcontainingthedataforthatrow.)

Finds the Point of Control (POC) row for the volume footprint represented by a footprint object, then returns the ID of a volume_row object containing the data for that row.
Syntax
footprint.poc(id) → volume_row
Arguments
id (footprint) The reference (ID) of the footprint object to analyze.
Returns
The ID of a volume_row object representing the footprint's POC row.
footprint.rows()

## [Creates an array containing all volume_row IDs from a footprint object. Each volume_row object referenced in the array contains data for one row in the calculated volume footprint, where the first object represents the lowest row and the last one represents the highest row.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createsanarraycontainingallvolume_rowIDsfromafootprintobject.Eachvolume_rowobjectreferencedinthearraycontainsdataforonerowinthecalculatedvolumefootprintwherethefirstobjectrepresentsthelowestrowandthelastonerepresentsthehighestrow.)

Creates an array containing all volume_row IDs from a footprint object. Each volume_row object referenced in the array contains data for one row in the calculated volume footprint, where the first object represents the lowest row and the last one represents the highest row.
Syntax
footprint.rows(id) → array<volume_row>
Arguments
id (footprint) The reference (ID) of the footprint object to analyze.
Returns
The ID of an array containing a volume_row ID for each row in the footprint.
footprint.sell_volume()

## [Calculates the total "sell" volume for the volume footprint represented by a footprint object.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculatesthetotalsellvolumeforthevolumefootprintrepresentedbyafootprintobject.)

Calculates the total "sell" volume for the volume footprint represented by a footprint object.
Syntax
footprint.sell_volume(id) → series float
Arguments
id (footprint) The reference (ID) of the footprint object to analyze.
Returns
The total "sell" volume measured by the footprint.
footprint.total_volume()

## [Calculates the sum of the total "buy" volume and "sell" volume for the volume footprint represented by a footprint object.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculatesthesumofthetotalbuyvolumeandsellvolumeforthevolumefootprintrepresentedbyafootprintobject.)

Calculates the sum of the total "buy" volume and "sell" volume for the volume footprint represented by a footprint object.
Syntax
footprint.total_volume(id) → series float
Arguments
id (footprint) The reference (ID) of the footprint object to analyze.
Returns
The total volume measured by the footprint.
footprint.vah()

## [Finds the Value Area High (VAH) row for the volume footprint represented by a footprint object, then returns the ID of a volume_row object containing the data for that row.](https://www.tradingview.com/pine-script-reference/v6/#fun_FindstheValueAreaHighVAHrowforthevolumefootprintrepresentedbyafootprintobjectthenreturnstheIDofavolume_rowobjectcontainingthedataforthatrow.)

Finds the Value Area High (VAH) row for the volume footprint represented by a footprint object, then returns the ID of a volume_row object containing the data for that row.
Syntax
footprint.vah(id) → volume_row
Arguments
id (footprint) The reference (ID) of the footprint object to analyze.
Returns
The ID of a volume_row object representing the footprint's VAH row.
footprint.val()

## [Finds the Value Area Low (VAL) row for the volume footprint represented by a footprint object, then returns the ID of a volume_row object containing the data for that row.](https://www.tradingview.com/pine-script-reference/v6/#fun_FindstheValueAreaLowVALrowforthevolumefootprintrepresentedbyafootprintobjectthenreturnstheIDofavolume_rowobjectcontainingthedataforthatrow.)

Finds the Value Area Low (VAL) row for the volume footprint represented by a footprint object, then returns the ID of a volume_row object containing the data for that row.
Syntax
footprint.val(id) → volume_row
Arguments
id (footprint) The reference (ID) of the footprint object to analyze.
Returns
The ID of a volume_row object representing the footprint's VAL row.
hline()

## [Renders a horizontal line at a given fixed price level.](https://www.tradingview.com/pine-script-reference/v6/#fun_Rendersahorizontallineatagivenfixedpricelevel.)

Renders a horizontal line at a given fixed price level.
Syntax
hline(price, title, color, linestyle, linewidth, editable, display) → hline
Arguments
price (input int/float) Price value at which the object will be rendered. Required argument.
title (const string) Title of the object.
color (input color) Color of the rendered line. Must be a constant value (not an expression). Optional argument.
linestyle (input hline_style) Style of the rendered line. Possible values are: hline.style_solid, hline.style_dotted, hline.style_dashed. Optional argument.
linewidth (input int) Width of the rendered line. Default value is 1.
editable (input bool) If true then hline style will be editable in Format dialog. Default is true.
display (input plot_simple_display) Controls where the hline is displayed. Possible values are: display.none, display.all. Default is display.all.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("input.hline", overlay=true)
hline(3.14, title='Pi', color=color.blue, linestyle=hline.style_dotted, linewidth=2)

## [// You may fill the background between any two hlines with a fill() function:](https://www.tradingview.com/pine-script-reference/v6/#fun_Youmayfillthebackgroundbetweenanytwohlineswithafillfunction)

// You may fill the background between any two hlines with a fill() function:
h1 = hline(20)
h2 = hline(10)
fill(h1, h2, color=color.new(color.green, 90))
Returns
An hline object, that can be used in fill()
See also
fill()
hour()

## [Syntax](https://www.tradingview.com/pine-script-reference/v6/#fun_Syntax)

Syntax
hour(time, timezone) → series int
Arguments
time (series int) UNIX time in milliseconds.
timezone (series string) Allows adjusting the returned value to a time zone specified in either UTC/GMT notation (e.g., "UTC-5", "GMT+0530") or as an IANA time zone database name (e.g., "America/New_York"). Optional. The default is syminfo.timezone.
Returns
Hour (in exchange timezone) for provided UNIX time.
Remarks
UNIX time is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
See also
hour
time()
year()
month()
dayofmonth()
dayofweek()
minute()
second()
indicator()

## [A declaration statement that identifies the script as an indicator and sets specific script-wide properties.](https://www.tradingview.com/pine-script-reference/v6/#fun_Adeclarationstatementthatidentifiesthescriptasanindicatorandsetsspecificscript-wideproperties.)

A declaration statement that identifies the script as an indicator and sets specific script-wide properties.
Syntax
indicator(title, shorttitle, overlay, format, precision, scale, max_bars_back, timeframe, timeframe_gaps, explicit_plot_zorder, max_lines_count, max_labels_count, max_boxes_count, calc_bars_count, max_polylines_count, dynamic_requests, behind_chart) → void
Arguments
title (const string) A string representing the script's title. The script displays the string's text in all possible locations if the declaration statement does not include a shorttitle argument. Additionally, the "Publish script" window uses the text as the default title for a script publication.
shorttitle (const string) Optional. A string representing the script's display name on charts. If specified and not an empty string, the value's text replaces the title string in most chart locations, including the "Settings" window, the script's status line, the Data Window, and the "Create alert" dialog box. Otherwise, the title string appears as the script's title in all locations. The default is an empty string.
overlay (const bool) Optional. If true, the script's visuals appear on the main chart pane if the user adds it to the chart directly, or in another script's pane if the user applies it to that script. If false, the script's visuals appear in a separate pane. However, if a function call that creates visuals includes force_overlay = true, its output always appears on the main chart pane, even if the script occupies a separate pane. Changes to this argument apply only after the user adds the script to the chart again. Additionally, if the user moves the script to another pane by selecting a "Move to" option in the script's "More" menu, the script does not move back to its original pane after any updates to the source code. The default is false.
format (const string) Optional. Specifies the format of the script's plotted values. Possible values are format.inherit, format.price, format.volume, and format.percent. The default is format.inherit.
precision (const int) Optional. Specifies the number of fractional digits that the script shows for plotted numbers. The value must be an integer from 0 to 16. If specified and the format argument is format.inherit, the script uses format.price as the formatting option instead. If the format argument is {format.volume}, the script ignores the precision value, because the decimal precision rules specified by format.volume supersede other precision settings. By default, the script inherits the precision settings of the chart.
scale (const scale_type) Optional. Determines the location of the script's price scale and the scaling behavior of the script's visuals. Possible values are scale.right, scale.left, and scale.none. If specified and the script overlays on the main chart pane or another script's pane, the script scales its visuals independently to fit the pane's visual space. If the script occupies the same pane as the main chart or another script, scale.right or scale.left adds a separate price scale for the script to the left or right side of that pane. If the script occupies a separate pane, either argument positions the price scale for that pane on the left or right side without adding a new scale. If the argument is scale.none, which is valid only if the overlay argument is true, the script displays plotted numbers directly on the scale of the existing pane, or displays values on a new price scale if the user moves it to a new pane. Changes to the argument apply only after the user adds the script to the chart again. If not specified, the script uses the main price scale for the pane it occupies, and it does not scale its visuals separately if it overlays on an existing pane.
max_bars_back (const int) Optional. Sets the minimum length of all the script's historical buffers, which determine the number of bars back that the script can reference for each series using the [] operator or the functions that retrieve history internally. The value must be an integer from 0 to 5000. By default, Pine's runtime system automatically calculates appropriate historical buffer sizes for each series while loading a script. Manually setting buffer sizes is necessary only in rare cases where automatic size detection fails. See the Historical buffers section of our User Manual for advanced details.
timeframe (const string) Optional. A valid timeframe string that determines the main timeframe the script uses for its calculations. If specified, the script automatically adds a "Timeframe" input to the "Settings/Inputs" tab. The input's displayed default in the tab represents the same timeframe as the specified argument. If the value is an empty string or not specified, the script uses the same timeframe as the chart. An argument is allowed for this parameter only if the script does not use drawing types or alert() function calls.
timeframe_gaps (const bool) Optional. Controls how the script displays plotted values if the timeframe value represents a higher timeframe than the chart's timeframe. An argument for this parameter is allowed only if the call includes a timeframe argument. If specified, the script adds a "Wait for timeframe closes" input, where users can change the setting, below the generated "Timeframe" input in the "Settings/Inputs" tab. If true, the indicator displays values only on the chart bars where new higher-timeframe data is available, and na on all other bars. If false, the indicator displays the last retrieved values on all chart bars where new data is not available. The default is true.
explicit_plot_zorder (const bool) Optional. Specifies which rules the script uses to determine the visual order of plots from plot*() calls, levels from hline() calls, and fills from fill() calls on the chart. If true, the indicator displays these visuals in the order of their function calls in the code. If false, the script uses the default z-index rules to determine the order of the visuals. The default is false.
max_lines_count (const int) Optional. Determines the maximum number of line objects that remain available to the script. The system automatically deletes the oldest line objects when the number of lines exceeds the limit. The limit specified by the argument is approximate; the script might display more drawings than specified. The default is ~50 lines.
max_labels_count (const int) Optional. Determines the maximum number of label objects that remain available to the script. The system automatically deletes the oldest label objects when the number of labels exceeds the limit. The limit specified by the argument is approximate; the script might display more drawings than specified. The default is ~50 labels.
max_boxes_count (const int) Optional. Determines the maximum number of box objects that remain available to the script. The system automatically deletes the oldest box objects when the number of boxes exceeds the limit. The limit specified by the argument is approximate; the script might display more drawings than specified. The default is ~50 boxes.
calc_bars_count (const int) Optional. Determines how many of the most recent historical bars are available to the script. If specified, the script automatically adds a "Calculated bars" input to the "Settings/Inputs" tab. If the value is positive and less than the number of historical bars in the dataset, the script starts its calculations that number of bars before the most recent bar. If the value is 0, the script's calculations start on the dataset's first bar. The default is 0.
max_polylines_count (const int) Optional. Determines the maximum number of polyline objects that remain available to the script. The system automatically deletes the oldest polyline objects when the number of polylines exceeds the limit. The limit specified by the argument is approximate; the script might display more drawings than specified. The default is ~50 polylines.
dynamic_requests (const bool) Optional. Specifies whether the script can use dynamic request.*() function calls. Dynamic request.*() calls are allowed within the local scopes of conditional structures (e.g., if), loops (e.g., for), and exported functions. Additionally, such calls allow "series" arguments for several parameters that otherwise require values with "simple" or weaker qualifiers. See the Dynamic requests section of our User Manual for more information. The default is true.
behind_chart (const bool) Optional. Controls whether all plots and drawings appear behind the chart display (if true) or in front of it (if false). This parameter takes effect only when the overlay argument is true. Changes to the argument apply only after the user adds the script to the chart again. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("My script", shorttitle="Script")
plot(close)
Remarks
Every indicator script must include exactly one indicator() statement in the code.
See also
strategy()
library()
input()6 overloads

## [Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function automatically detects the type of the argument used for 'defval' and uses the corresponding input widget.](https://www.tradingview.com/pine-script-reference/v6/#fun_AddsaninputtotheInputstabofyourscriptsSettingswhichallowsyoutoprovideconfigurationoptionstoscriptusers.Thisfunctionautomaticallydetectsthetypeoftheargumentusedfordefvalandusesthecorrespondinginputwidget.)

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function automatically detects the type of the argument used for 'defval' and uses the corresponding input widget.
Syntax & Overloads
input(defval, title, tooltip, inline, group, display, active) → input color
input(defval, title, tooltip, inline, group, display, active) → input string
input(defval, title, tooltip, inline, group, display, active) → input int
input(defval, title, tooltip, inline, group, display, active) → input float
input(defval, title, inline, group, tooltip, display, active) → series float
input(defval, title, tooltip, inline, group, display, active) → input bool
Arguments
defval (const int/float/bool/string/color or source-type built-ins) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where script users can change it. Source-type built-ins are built-in series float variables that specify the source of the calculation: close, hlc3, etc.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
display (const plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default depends on the type of the value passed to defval: display.none for bool and color values, display.all for everything else.
active (input bool) Optional. Specifies whether users can change the value of the input in the script's "Settings/Inputs" tab. The script can use this parameter to set the state of the input based on the values of other inputs. If true, users can change the value of the input. If false, the input is grayed out, and users cannot change the value. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("input", overlay=true)
i_switch = input(true, "On/Off")
plot(i_switch ? open : na)

## [i_len = input(7, "Length")](https://www.tradingview.com/pine-script-reference/v6/#fun_i_leninput7Length)

i_len = input(7, "Length")
i_src = input(close, "Source")
plot(ta.sma(i_src, i_len))

## [i_border = input(142.50, "Price Border")](https://www.tradingview.com/pine-script-reference/v6/#fun_i_borderinput142.50PriceBorder)

i_border = input(142.50, "Price Border")
hline(i_border)
bgcolor(close > i_border ? color.green : color.red)

## [i_col = input(color.red, "Plot Color")](https://www.tradingview.com/pine-script-reference/v6/#fun_i_colinputcolor.redPlotColor)

i_col = input(color.red, "Plot Color")
plot(close, color=i_col)

## [i_text = input("Hello!", "Message")](https://www.tradingview.com/pine-script-reference/v6/#fun_i_textinputHelloMessage)

i_text = input("Hello!", "Message")
l = label.new(bar_index, high, text=i_text)
label.delete(l[1])
Returns
Value of input variable.
Remarks
Result of input() function always should be assigned to a variable, see examples above.
See also
input.bool()
input.color()
input.int()
input.float()
input.string()
input.symbol()
input.timeframe()
input.text_area()
input.session()
input.source()
input.time()
input.bool()

## [Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a checkmark to the script's inputs.](https://www.tradingview.com/pine-script-reference/v6/#fun_AddsaninputtotheInputstabofyourscriptsSettingswhichallowsyoutoprovideconfigurationoptionstoscriptusers.Thisfunctionaddsacheckmarktothescriptsinputs.)

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a checkmark to the script's inputs.
Syntax
input.bool(defval, title, tooltip, inline, group, confirm, display, active) → input bool
Arguments
defval (const bool) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (const plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.none.
active (input bool) Optional. Specifies whether users can change the value of the input in the script's "Settings/Inputs" tab. The script can use this parameter to set the state of the input based on the values of other inputs. If true, users can change the value of the input. If false, the input is grayed out, and users cannot change the value. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("input.bool", overlay=true)
i_switch = input.bool(true, "On/Off")
plot(i_switch ? open : na)
Returns
Value of input variable.
Remarks
Result of input.bool() function always should be assigned to a variable, see examples above.
See also
input.int()
input.float()
input.string()
input.text_area()
input.symbol()
input.timeframe()
input.session()
input.source()
input.color()
input.time()
input()
input.color()

## [Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a color picker that allows the user to select a color and transparency, either from a palette or a hex value.](https://www.tradingview.com/pine-script-reference/v6/#fun_AddsaninputtotheInputstabofyourscriptsSettingswhichallowsyoutoprovideconfigurationoptionstoscriptusers.Thisfunctionaddsacolorpickerthatallowstheusertoselectacolorandtransparencyeitherfromapaletteorahexvalue.)

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a color picker that allows the user to select a color and transparency, either from a palette or a hex value.
Syntax
input.color(defval, title, tooltip, inline, group, confirm, display, active) → input color
Arguments
defval (const color) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (const plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.none.
active (input bool) Optional. Specifies whether users can change the value of the input in the script's "Settings/Inputs" tab. The script can use this parameter to set the state of the input based on the values of other inputs. If true, users can change the value of the input. If false, the input is grayed out, and users cannot change the value. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("input.color", overlay=true)
i_col = input.color(color.red, "Plot Color")
plot(close, color=i_col)
Returns
Value of input variable.
Remarks
Result of input.color() function always should be assigned to a variable, see examples above.
See also
input.bool()
input.int()
input.float()
input.string()
input.text_area()
input.symbol()
input.timeframe()
input.session()
input.source()
input.time()
input()
input.enum()

## [Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a dropdown with options based on the enum fields passed to its defval and options parameters.](https://www.tradingview.com/pine-script-reference/v6/#fun_AddsaninputtotheInputstabofyourscriptsSettingswhichallowsyoutoprovideconfigurationoptionstoscriptusers.Thisfunctionaddsadropdownwithoptionsbasedontheenumfieldspassedtoitsdefvalandoptionsparameters.)

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a dropdown with options based on the enum fields passed to its defval and options parameters.
The text for each option in the resulting dropdown corresponds to the titles of the included fields. If a field's title is not specified in the enum declaration, its title is the string representation of its name.
Syntax
input.enum(defval, title, options, tooltip, inline, group, confirm, display, active) → input enum
Arguments
defval (const enum) Determines the default value of the input, which users can change in the script's "Settings/Inputs" tab. When the options parameter has a specified tuple of enum fields, the tuple must include the defval.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
options (tuple of enum fields: [enumName.field1, enumName.field2, ...]) A list of options to choose from. Optional. By default, the titles of all of the enum's fields are available in the dropdown. Passing a tuple as the options argument limits the list to only the included fields.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (const plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.all.
active (input bool) Optional. Specifies whether users can change the value of the input in the script's "Settings/Inputs" tab. The script can use this parameter to set the state of the input based on the values of other inputs. If true, users can change the value of the input. If false, the input is grayed out, and users cannot change the value. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("Session highlight", overlay = true)

## [//@enum        Contains fields with popular timezones as titles.](https://www.tradingview.com/pine-script-reference/v6/#fun_enumContainsfieldswithpopulartimezonesastitles.)

//@enum        Contains fields with popular timezones as titles.
//@field exch  Has an empty string as the title to represent the chart timezone.
enum tz
    utc  = "UTC"
    exch = ""
    ny   = "America/New_York"
    chi  = "America/Chicago"
    lon  = "Europe/London"
    tok  = "Asia/Tokyo"

## [//@variable The session string.](https://www.tradingview.com/pine-script-reference/v6/#fun_variableThesessionstring.)

//@variable The session string.
selectedSession = input.session("1200-1500", "Session")
//@variable The selected timezone. The input's dropdown contains the fields in the `tz` enum.
selectedTimezone = input.enum(tz.utc, "Session Timezone")

## [//@variable Is `true` if the current bar's time is in the specified session.](https://www.tradingview.com/pine-script-reference/v6/#fun_variableIstrueifthecurrentbarstimeisinthespecifiedsession.)

//@variable Is `true` if the current bar's time is in the specified session.
bool inSession = false
if not na(time("", selectedSession, str.tostring(selectedTimezone)))
    inSession := true

## [// Highlight the background when `inSession` is `true`.](https://www.tradingview.com/pine-script-reference/v6/#fun_HighlightthebackgroundwheninSessionistrue.)

// Highlight the background when `inSession` is `true`.
bgcolor(inSession ? color.new(color.green, 90) : na, title = "Active session highlight")
Returns
Value of input variable.
Remarks
All fields included in the defval and options arguments must belong to the same enum.
See also
input.text_area()
input.bool()
input.int()
input.float()
input.symbol()
input.timeframe()
input.session()
input.source()
input.color()
input.time()
input()
input.float()2 overloads

## [Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a field for a float input to the script's inputs.](https://www.tradingview.com/pine-script-reference/v6/#fun_AddsaninputtotheInputstabofyourscriptsSettingswhichallowsyoutoprovideconfigurationoptionstoscriptusers.Thisfunctionaddsafieldforafloatinputtothescriptsinputs.)

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a field for a float input to the script's inputs.
Syntax & Overloads
input.float(defval, title, options, tooltip, inline, group, confirm, display, active) → input float
input.float(defval, title, minval, maxval, step, tooltip, inline, group, confirm, display, active) → input float
Arguments
defval (const int/float) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where script users can change it. When a list of values is used with the options parameter, the value must be one of them.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
options (tuple of const int/float values: [val1, val2, ...]) A list of options to choose from a dropdown menu, separated by commas and enclosed in square brackets: [val1, val2, ...]. When using this parameter, the minval, maxval and step parameters cannot be used.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (const plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.all.
active (input bool) Optional. Specifies whether users can change the value of the input in the script's "Settings/Inputs" tab. The script can use this parameter to set the state of the input based on the values of other inputs. If true, users can change the value of the input. If false, the input is grayed out, and users cannot change the value. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("input.float", overlay=true)
i_angle1 = input.float(0.5, "Sin Angle", minval=-3.14, maxval=3.14, step=0.02)
plot(math.sin(i_angle1) > 0 ? close : open, "sin", color=color.green)

## [i_angle2 = input.float(0, "Cos Angle", options=[-3.14, -1.57, 0, 1.57, 3.14])](https://www.tradingview.com/pine-script-reference/v6/#fun_i_angle2input.float0CosAngleoptions-3.14-1.5701.573.14)

i_angle2 = input.float(0, "Cos Angle", options=[-3.14, -1.57, 0, 1.57, 3.14])
plot(math.cos(i_angle2) > 0 ? close : open, "cos", color=color.red)
Returns
Value of input variable.
Remarks
Result of input.float() function always should be assigned to a variable, see examples above.
See also
input.bool()
input.int()
input.string()
input.text_area()
input.symbol()
input.timeframe()
input.session()
input.source()
input.color()
input.time()
input()
input.int()2 overloads

## [Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a field for an integer input to the script's inputs.](https://www.tradingview.com/pine-script-reference/v6/#fun_AddsaninputtotheInputstabofyourscriptsSettingswhichallowsyoutoprovideconfigurationoptionstoscriptusers.Thisfunctionaddsafieldforanintegerinputtothescriptsinputs.)

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a field for an integer input to the script's inputs.
Syntax & Overloads
input.int(defval, title, options, tooltip, inline, group, confirm, display, active) → input int
input.int(defval, title, minval, maxval, step, tooltip, inline, group, confirm, display, active) → input int
Arguments
defval (const int) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where script users can change it. When a list of values is used with the options parameter, the value must be one of them.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
options (tuple of const int values: [val1, val2, ...]) A list of options to choose from a dropdown menu, separated by commas and enclosed in square brackets: [val1, val2, ...]. When using this parameter, the minval, maxval and step parameters cannot be used.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (const plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.all.
active (input bool) Optional. Specifies whether users can change the value of the input in the script's "Settings/Inputs" tab. The script can use this parameter to set the state of the input based on the values of other inputs. If true, users can change the value of the input. If false, the input is grayed out, and users cannot change the value. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("input.int", overlay=true)
i_len1 = input.int(10, "Length 1", minval=5, maxval=21, step=1)
plot(ta.sma(close, i_len1))

## [i_len2 = input.int(10, "Length 2", options=[5, 10, 21])](https://www.tradingview.com/pine-script-reference/v6/#fun_i_len2input.int10Length2options51021)

i_len2 = input.int(10, "Length 2", options=[5, 10, 21])
plot(ta.sma(close, i_len2))
Returns
Value of input variable.
Remarks
Result of input.int() function always should be assigned to a variable, see examples above.
See also
input.bool()
input.float()
input.string()
input.text_area()
input.symbol()
input.timeframe()
input.session()
input.source()
input.color()
input.time()
input()
input.price()

## [Adds a price input to the script's "Settings/Inputs" tab. The user can change the price in the settings or by selecting the indicator and dragging the price line.](https://www.tradingview.com/pine-script-reference/v6/#fun_AddsapriceinputtothescriptsSettingsInputstab.Theusercanchangethepriceinthesettingsorbyselectingtheindicatoranddraggingthepriceline.)

Adds a price input to the script's "Settings/Inputs" tab. The user can change the price in the settings or by selecting the indicator and dragging the price line.
Syntax
input.price(defval, title, tooltip, inline, group, confirm, display, active) → input float
Arguments
defval (const int/float) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) Optional. If true, the script prompts the user to set the input's initial value by clicking a point on the chart. If inputs of other types require confirmation, the "Confirm inputs" dialog box also displays this input's field, allowing final adjustments to the value before the script starts to run. The default is false.
display (const plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.all.
active (input bool) Optional. Specifies whether users can change the value of the input in the script's "Settings/Inputs" tab. The script can use this parameter to set the state of the input based on the values of other inputs. If true, users can change the value of the input. If false, the input is grayed out, and users cannot change the value. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("input.price", overlay=true)
price1 = input.price(title="Date", defval=42)
plot(price1)

## [price2 = input.price(54, title="Date")](https://www.tradingview.com/pine-script-reference/v6/#fun_price2input.price54titleDate)

price2 = input.price(54, title="Date")
plot(price2)
Returns
Value of input variable.
Remarks
The user can change the input's value by specifying a new value in the "Settings/Inputs" tab, or by moving the input's marker on the chart. Alternatively, they can select "Reset points" from the script's "More" menu and set a new input value by clicking a point on the chart.
If an input.time() and input.price() function call in the script share a unique inline argument and have matching group arguments, those calls create a single interactive point marker on the chart. The user can move that marker to adjust the input time and price values simultaneously.
See also
input.bool()
input.int()
input.float()
input.string()
input.text_area()
input.symbol()
input.resolution()
input.session()
input.source()
input.color()
input()
input.session()

## [Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds two dropdowns that allow the user to specify the beginning and the end of a session using the session selector and returns the result as a string.](https://www.tradingview.com/pine-script-reference/v6/#fun_AddsaninputtotheInputstabofyourscriptsSettingswhichallowsyoutoprovideconfigurationoptionstoscriptusers.Thisfunctionaddstwodropdownsthatallowtheusertospecifythebeginningandtheendofasessionusingthesessionselectorandreturnstheresultasastring.)

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds two dropdowns that allow the user to specify the beginning and the end of a session using the session selector and returns the result as a string.
Syntax
input.session(defval, title, options, tooltip, inline, group, confirm, display, active) → input string
Arguments
defval (const string) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it. When a list of values is used with the options parameter, the value must be one of them.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
options (tuple of const string values: [val1, val2, ...]) A list of options to choose from.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (const plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.all.
active (input bool) Optional. Specifies whether users can change the value of the input in the script's "Settings/Inputs" tab. The script can use this parameter to set the state of the input based on the values of other inputs. If true, users can change the value of the input. If false, the input is grayed out, and users cannot change the value. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("input.session", overlay=true)
i_sess = input.session("1300-1700", "Session", options=["0930-1600", "1300-1700", "1700-2100"])
t = time(timeframe.period, i_sess)
bgcolor(time == t ? color.green : na)
Returns
Value of input variable.
Remarks
Result of input.session() function always should be assigned to a variable, see examples above.
See also
input.bool()
input.int()
input.float()
input.string()
input.text_area()
input.symbol()
input.timeframe()
input.source()
input.color()
input.time()
input()
input.source()

## [Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a dropdown that allows the user to select a source for the calculation, e.g. close, hl2, etc. The user can also select an output from another indicator on their chart as the source.](https://www.tradingview.com/pine-script-reference/v6/#fun_AddsaninputtotheInputstabofyourscriptsSettingswhichallowsyoutoprovideconfigurationoptionstoscriptusers.Thisfunctionaddsadropdownthatallowstheusertoselectasourceforthecalculatione.g.closehl2etc.Theusercanalsoselectanoutputfromanotherindicatorontheirchartasthesource.)

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a dropdown that allows the user to select a source for the calculation, e.g. close, hl2, etc. The user can also select an output from another indicator on their chart as the source.
Syntax
input.source(defval, title, tooltip, inline, group, display, active, confirm) → series float
Arguments
defval (open/high/low/close/hl2/hlc3/ohlc4/hlcc4) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
display (const plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.all.
active (input bool) Optional. Specifies whether users can change the value of the input in the script's "Settings/Inputs" tab. The script can use this parameter to set the state of the input based on the values of other inputs. If true, users can change the value of the input. If false, the input is grayed out, and users cannot change the value. The default is true.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("input.source", overlay=true)
i_src = input.source(close, "Source")
plot(i_src)
Returns
Value of input variable.
Remarks
Result of input.source() function always should be assigned to a variable, see examples above.
See also
input.bool()
input.int()
input.float()
input.string()
input.text_area()
input.symbol()
input.timeframe()
input.session()
input.color()
input.time()
input()
input.string()

## [Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a field for a string input to the script's inputs.](https://www.tradingview.com/pine-script-reference/v6/#fun_AddsaninputtotheInputstabofyourscriptsSettingswhichallowsyoutoprovideconfigurationoptionstoscriptusers.Thisfunctionaddsafieldforastringinputtothescriptsinputs.)

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a field for a string input to the script's inputs.
Syntax
input.string(defval, title, options, tooltip, inline, group, confirm, display, active) → input string
Arguments
defval (const string) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it. When a list of values is used with the options parameter, the value must be one of them.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
options (tuple of const string values: [val1, val2, ...]) A list of options to choose from.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (const plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.all.
active (input bool) Optional. Specifies whether users can change the value of the input in the script's "Settings/Inputs" tab. The script can use this parameter to set the state of the input based on the values of other inputs. If true, users can change the value of the input. If false, the input is grayed out, and users cannot change the value. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("input.string", overlay=true)
i_text = input.string("Hello!", "Message")
l = label.new(bar_index, high, i_text)
label.delete(l[1])
Returns
Value of input variable.
Remarks
Result of input.string() function always should be assigned to a variable, see examples above.
See also
input.text_area()
input.bool()
input.int()
input.float()
input.symbol()
input.timeframe()
input.session()
input.source()
input.color()
input.time()
input()
input.symbol()

## [Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a field that allows the user to select a specific symbol using the symbol search and returns that symbol, paired with its exchange prefix, as a string.](https://www.tradingview.com/pine-script-reference/v6/#fun_AddsaninputtotheInputstabofyourscriptsSettingswhichallowsyoutoprovideconfigurationoptionstoscriptusers.Thisfunctionaddsafieldthatallowstheusertoselectaspecificsymbolusingthesymbolsearchandreturnsthatsymbolpairedwithitsexchangeprefixasastring.)

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a field that allows the user to select a specific symbol using the symbol search and returns that symbol, paired with its exchange prefix, as a string.
Syntax
input.symbol(defval, title, tooltip, inline, group, confirm, display, active) → input string
Arguments
defval (const string) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (const plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.all.
active (input bool) Optional. Specifies whether users can change the value of the input in the script's "Settings/Inputs" tab. The script can use this parameter to set the state of the input based on the values of other inputs. If true, users can change the value of the input. If false, the input is grayed out, and users cannot change the value. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("input.symbol", overlay=true)
i_sym = input.symbol("DELL", "Symbol")
s = request.security(i_sym, 'D', close)
plot(s)
Returns
Value of input variable.
Remarks
Result of input.symbol() function always should be assigned to a variable, see examples above.
See also
input.bool()
input.int()
input.float()
input.string()
input.text_area()
input.timeframe()
input.session()
input.source()
input.color()
input.time()
input()
input.text_area()

## [Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a field for a multiline text input.](https://www.tradingview.com/pine-script-reference/v6/#fun_AddsaninputtotheInputstabofyourscriptsSettingswhichallowsyoutoprovideconfigurationoptionstoscriptusers.Thisfunctionaddsafieldforamultilinetextinput.)

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a field for a multiline text input.
Syntax
input.text_area(defval, title, tooltip, group, confirm, display, active) → input string
Arguments
defval (const string) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (const plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.none.
active (input bool) Optional. Specifies whether users can change the value of the input in the script's "Settings/Inputs" tab. The script can use this parameter to set the state of the input based on the values of other inputs. If true, users can change the value of the input. If false, the input is grayed out, and users cannot change the value. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("input.text_area")
i_text = input.text_area(defval = "Hello \nWorld!", title = "Message")
plot(close)
Returns
Value of input variable.
Remarks
Result of input.text_area() function always should be assigned to a variable, see examples above.
See also
input.string()
input.bool()
input.int()
input.float()
input.symbol()
input.timeframe()
input.session()
input.source()
input.color()
input.time()
input()
input.time()

## [Adds two inputs to the script's "Settings/Inputs" tab on the same line: one for the date and one for the time. The user can change the price in the settings or by selecting the indicator and dragging the price line. The function returns a date/time value in UNIX format.](https://www.tradingview.com/pine-script-reference/v6/#fun_AddstwoinputstothescriptsSettingsInputstabonthesamelineoneforthedateandoneforthetime.Theusercanchangethepriceinthesettingsorbyselectingtheindicatoranddraggingthepriceline.ThefunctionreturnsadatetimevalueinUNIXformat.)

Adds two inputs to the script's "Settings/Inputs" tab on the same line: one for the date and one for the time. The user can change the price in the settings or by selecting the indicator and dragging the price line. The function returns a date/time value in UNIX format.
Syntax
input.time(defval, title, tooltip, inline, group, confirm, display, active) → input int
Arguments
defval (const int) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it. The value can be a timestamp() function, but only if it uses a date argument in const string format.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) Optional. If true, the script prompts the user to set the input's initial value by clicking a point on the chart. If inputs of other types require confirmation, the "Confirm inputs" dialog box also displays this input's field, allowing final adjustments to the value before the script starts to run. The default is false.
display (const plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.none.
active (input bool) Optional. Specifies whether users can change the value of the input in the script's "Settings/Inputs" tab. The script can use this parameter to set the state of the input based on the values of other inputs. If true, users can change the value of the input. If false, the input is grayed out, and users cannot change the value. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("input.time", overlay=true)
i_date = input.time(timestamp("20 Jul 2021 00:00 +0300"), "Date")
l = label.new(i_date, high, "Date", xloc=xloc.bar_time)
label.delete(l[1])
Returns
Value of input variable.
Remarks
The user can change the input's value by specifying a new value in the "Settings/Inputs" tab, or by moving the input's marker on the chart. Alternatively, they can select "Reset points" from the script's "More" menu and set a new input value by clicking a point on the chart.
If an input.time() and input.price() function call in the script share a unique inline argument and have matching group arguments, those calls create a single interactive point marker on the chart. The user can move that marker to adjust the input time and price values simultaneously.
See also
input.bool()
input.int()
input.float()
input.string()
input.text_area()
input.symbol()
input.timeframe()
input.session()
input.source()
input.color()
input()
input.timeframe()

## [Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a dropdown that allows the user to select a specific timeframe via the timeframe selector and returns it as a string. The selector includes the custom timeframes a user may have added using the chart's Timeframe dropdown.](https://www.tradingview.com/pine-script-reference/v6/#fun_AddsaninputtotheInputstabofyourscriptsSettingswhichallowsyoutoprovideconfigurationoptionstoscriptusers.Thisfunctionaddsadropdownthatallowstheusertoselectaspecifictimeframeviathetimeframeselectorandreturnsitasastring.TheselectorincludesthecustomtimeframesausermayhaveaddedusingthechartsTimeframedropdown.)

Adds an input to the Inputs tab of your script's Settings, which allows you to provide configuration options to script users. This function adds a dropdown that allows the user to select a specific timeframe via the timeframe selector and returns it as a string. The selector includes the custom timeframes a user may have added using the chart's Timeframe dropdown.
Syntax
input.timeframe(defval, title, options, tooltip, inline, group, confirm, display, active) → input string
Arguments
defval (const string) Determines the default value of the input variable proposed in the script's "Settings/Inputs" tab, from where the user can change it. When a list of values is used with the options parameter, the value must be one of them.
title (const string) Title of the input. If not specified, the variable name is used as the input's title. If the title is specified, but it is empty, the name will be an empty string.
options (tuple of const string values: [val1, val2, ...]) A list of options to choose from.
tooltip (const string) The string that will be shown to the user when hovering over the tooltip icon.
inline (const string) Combines all the input calls using the same argument in one line. The string used as an argument is not displayed. It is only used to identify inputs belonging to the same line.
group (const string) Creates a header above all inputs using the same group argument string. The string is also used as the header's text.
confirm (const bool) If true, then user will be asked to confirm input value before indicator is added to chart. Default value is false.
display (const plot_display) Controls where the script will display the input's information, aside from within the script's settings. This option allows one to remove a specific input from the script's status line or the Data Window to ensure only the most necessary inputs are displayed there. Possible values: display.none, display.data_window, display.status_line, display.all. Optional. The default is display.all.
active (input bool) Optional. Specifies whether users can change the value of the input in the script's "Settings/Inputs" tab. The script can use this parameter to set the state of the input based on the values of other inputs. If true, users can change the value of the input. If false, the input is grayed out, and users cannot change the value. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("input.timeframe", overlay=true)
i_res = input.timeframe('D', "Resolution", options=['D', 'W', 'M'])
s = request.security("AAPL", i_res, close)
plot(s)
Returns
Value of input variable.
Remarks
Result of input.timeframe() function always should be assigned to a variable, see examples above.
See also
input.bool()
input.int()
input.float()
input.string()
input.text_area()
input.symbol()
input.session()
input.source()
input.color()
input.time()
input()
int()4 overloads

## [Casts na or truncates float value to int](https://www.tradingview.com/pine-script-reference/v6/#fun_Castsnaortruncatesfloatvaluetoint)

Casts na or truncates float value to int
Syntax & Overloads
int(x) → const int
int(x) → input int
int(x) → simple int
int(x) → series int
Arguments
x (const int/float) The value to convert to the specified type, usually na.
Returns
The value of the argument after casting to int.
See also
float()
bool()
color()
string()
line()
label()
label()

## [Casts na to label](https://www.tradingview.com/pine-script-reference/v6/#fun_Castsnatolabel)

Casts na to label
Syntax
label(x) → series label
Arguments
x (series label) The value to convert to the specified type, usually na.
Returns
The value of the argument after casting to label.
See also
float()
int()
bool()
color()
string()
line()
label.copy()

## [Clones the label object.](https://www.tradingview.com/pine-script-reference/v6/#fun_Clonesthelabelobject.)

Clones the label object.
Syntax
label.copy(id) → series label
Arguments
id (series label) Label object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator('Last 100 bars highest/lowest', overlay = true)
LOOKBACK = 100
highest = ta.highest(LOOKBACK)
highestBars = ta.highestbars(LOOKBACK)
lowest = ta.lowest(LOOKBACK)
lowestBars = ta.lowestbars(LOOKBACK)
if barstate.islastconfirmedhistory
    var labelHigh = label.new(bar_index + highestBars, highest, str.tostring(highest), color = color.green)
    var labelLow = label.copy(labelHigh)
    label.set_xy(labelLow, bar_index + lowestBars, lowest)
    label.set_text(labelLow, str.tostring(lowest))
    label.set_color(labelLow, color.red)
    label.set_style(labelLow, label.style_label_up)
Returns
New label ID object which may be passed to label.setXXX and label.getXXX functions.
See also
label.new()
label.delete()
label.delete()

## [Deletes the specified label object. If it has already been deleted, does nothing.](https://www.tradingview.com/pine-script-reference/v6/#fun_Deletesthespecifiedlabelobject.Ifithasalreadybeendeleteddoesnothing.)

Deletes the specified label object. If it has already been deleted, does nothing.
Syntax
label.delete(id) → void
Arguments
id (series label) Label object to delete.
See also
label.new()
label.get_text()

## [Returns the text of this label object.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthetextofthislabelobject.)

Returns the text of this label object.
Syntax
label.get_text(id) → series string
Arguments
id (series label) Label object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("label.get_text")
my_label = label.new(time, open, text="Open bar text", xloc=xloc.bar_time)
a = label.get_text(my_label)
label.new(time, close, text = a + " new", xloc=xloc.bar_time)
Returns
String object containing the text of this label.
See also
label.new()
label.get_x()

## [Returns UNIX time or bar index (depending on the last xloc value set) of this label's position.](https://www.tradingview.com/pine-script-reference/v6/#fun_ReturnsUNIXtimeorbarindexdependingonthelastxlocvaluesetofthislabelsposition.)

Returns UNIX time or bar index (depending on the last xloc value set) of this label's position.
Syntax
label.get_x(id) → series int
Arguments
id (series label) Label object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("label.get_x")
my_label = label.new(time, open, text="Open bar text", xloc=xloc.bar_time)
a = label.get_x(my_label)
plot(time - label.get_x(my_label)) //draws zero plot
Returns
UNIX timestamp (in milliseconds) or bar index.
See also
label.new()
label.get_y()

## [Returns price of this label's position.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnspriceofthislabelsposition.)

Returns price of this label's position.
Syntax
label.get_y(id) → series float
Arguments
id (series label) Label object.
Returns
Floating point value representing price.
See also
label.new()
label.new()2 overloads

## [Creates new label object.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createsnewlabelobject.)

Creates new label object.
Syntax & Overloads
label.new(point, text, xloc, yloc, color, style, textcolor, size, textalign, tooltip, text_font_family, force_overlay, text_formatting) → series label
label.new(x, y, text, xloc, yloc, color, style, textcolor, size, textalign, tooltip, text_font_family, force_overlay, text_formatting) → series label
Arguments
point (chart.point) A chart.point object that specifies the label's location.
text (series string) Label text. Default is empty string.
xloc (series string) See description of x argument. Possible values: xloc.bar_index and xloc.bar_time. Default is xloc.bar_index.
yloc (series string) Possible values are yloc.price, yloc.abovebar, yloc.belowbar. If yloc=yloc.price, y argument specifies the price of the label position. If yloc=yloc.abovebar, label is located above bar. If yloc=yloc.belowbar, label is located below bar. Default is yloc.price.
color (series color) Color of the label border and arrow
style (series string) Label style. Possible values: label.style_none, label.style_xcross, label.style_cross, label.style_triangleup, label.style_triangledown, label.style_flag, label.style_circle, label.style_arrowup, label.style_arrowdown, label.style_label_up, label.style_label_down, label.style_label_left, label.style_label_right, label.style_label_lower_left, label.style_label_lower_right, label.style_label_upper_left, label.style_label_upper_right, label.style_label_center, label.style_square, label.style_diamond, label.style_text_outline. Default is label.style_label_down.
textcolor (series color) Text color.
size (series int/string) Optional. Size of the label. Accepts a positive int value or one of the built-in size.* constants. The constants and their equivalent numeric sizes are: size.auto (0), size.tiny (~7), size.small (~10), size.normal (12), size.large (18), size.huge (24). The default value is size.normal, which represents the numeric size of 12.
textalign (series string) Label text alignment. Possible values: text.align_left, text.align_center, text.align_right. Default value is text.align_center.
tooltip (series string) Hover to see tooltip label.
text_font_family (series string) The font family of the text. Optional. The default value is font.family_default. Possible values: font.family_default, font.family_monospace.
force_overlay (const bool) If true, the drawing will display on the main chart pane, even when the script occupies a separate pane. Optional. The default is false.
text_formatting (series text_format) The formatting of the displayed text. Formatting options support addition. For example, text.format_bold + text.format_italic will make the text both bold and italicized. Possible values: text.format_none, text.format_bold, text.format_italic. Optional. The default is text.format_none.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("label.new")
var label1 = label.new(bar_index, low, text="Hello, world!", style=label.style_circle)
label.set_x(label1, 0)
label.set_xloc(label1, time, xloc.bar_time)
label.set_color(label1, color.red)
label.set_size(label1, size.large)
Returns
Label ID object which may be passed to label.setXXX and label.getXXX functions.
See also
label.delete()
label.set_x()
label.set_y()
label.set_xy()
label.set_xloc()
label.set_yloc()
label.set_color()
label.set_textcolor()
label.set_style()
label.set_size()
label.set_textalign()
label.set_tooltip()
label.set_text()
label.set_text_formatting()
label.set_color()

## [Sets label border and arrow color.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setslabelborderandarrowcolor.)

Sets label border and arrow color.
Syntax
label.set_color(id, color) → void
Arguments
id (series label) Label object.
color (series color) New label border and arrow color.
See also
label.new()
label.set_point()

## [Sets the location of the id label to point.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsthelocationoftheidlabeltopoint.)

Sets the location of the id label to point.
Syntax
label.set_point(id, point) → void
Arguments
id (series label) A label object.
point (chart.point) A chart.point object.
label.set_size()

## [Sets arrow and text size of the specified label object.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsarrowandtextsizeofthespecifiedlabelobject.)

Sets arrow and text size of the specified label object.
Syntax
label.set_size(id, size) → void
Arguments
id (series label) Label object.
size (series int/string) Size of the label. Accepts a positive int value or one of the built-in size.* constants. The constants and their equivalent numeric sizes are: size.auto (0), size.tiny (~7), size.small (~10), size.normal (12), size.large (18), size.huge (24). The default value is size.normal, which represents the numeric size of 12.
See also
size.auto
size.tiny
size.small
size.normal
size.large
size.huge
label.new()
label.set_style()

## [Sets label style.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setslabelstyle.)

Sets label style.
Syntax
label.set_style(id, style) → void
Arguments
id (series label) Label object.
style (series string) New label style. Possible values: label.style_none, label.style_xcross, label.style_cross, label.style_triangleup, label.style_triangledown, label.style_flag, label.style_circle, label.style_arrowup, label.style_arrowdown, label.style_label_up, label.style_label_down, label.style_label_left, label.style_label_right, label.style_label_lower_left, label.style_label_lower_right, label.style_label_upper_left, label.style_label_upper_right, label.style_label_center, label.style_square, label.style_diamond, label.style_text_outline.
See also
label.new()
label.set_text()

## [Sets label text](https://www.tradingview.com/pine-script-reference/v6/#fun_Setslabeltext)

Sets label text
Syntax
label.set_text(id, text) → void
Arguments
id (series label) Label object.
text (series string) New label text.
See also
label.new()
label.set_text_formatting()
label.set_text_font_family()

## [The function sets the font family of the text inside the label.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionsetsthefontfamilyofthetextinsidethelabel.)

The function sets the font family of the text inside the label.
Syntax
label.set_text_font_family(id, text_font_family) → void
Arguments
id (series label) A label object.
text_font_family (series string) The font family of the text. Possible values: font.family_default, font.family_monospace.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("Example of setting the label font")
if barstate.islastconfirmedhistory
    l = label.new(bar_index, 0, "monospace", yloc=yloc.abovebar)
    label.set_text_font_family(l, font.family_monospace)
See also
label.new()
font.family_default
font.family_monospace
label.set_text_formatting()

## [Sets the formatting attributes the drawing applies to displayed text.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setstheformattingattributesthedrawingappliestodisplayedtext.)

Sets the formatting attributes the drawing applies to displayed text.
Syntax
label.set_text_formatting(id, text_formatting) → void
Arguments
id (series label) Label object.
text_formatting (series text_format) The formatting of the displayed text. Formatting options support addition. For example, text.format_bold + text.format_italic will make the text both bold and italicized. Possible values: text.format_none, text.format_bold, text.format_italic. Optional. The default is text.format_none.
See also
label.new()
label.set_text()
label.set_textalign()

## [Sets the alignment for the label text.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsthealignmentforthelabeltext.)

Sets the alignment for the label text.
Syntax
label.set_textalign(id, textalign) → void
Arguments
id (series label) Label object.
textalign (series string) Label text alignment. Possible values: text.align_left, text.align_center, text.align_right.
See also
text.align_left
text.align_center
text.align_right
label.new()
label.set_textcolor()

## [Sets color of the label text.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setscolorofthelabeltext.)

Sets color of the label text.
Syntax
label.set_textcolor(id, textcolor) → void
Arguments
id (series label) Label object.
textcolor (series color) New text color.
See also
label.new()
label.set_tooltip()

## [Sets the tooltip text.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsthetooltiptext.)

Sets the tooltip text.
Syntax
label.set_tooltip(id, tooltip) → void
Arguments
id (series label) Label object.
tooltip (series string) Tooltip text.
See also
label.new()
label.set_x()

## [Sets bar index or bar time (depending on the xloc) of the label position.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsbarindexorbartimedependingonthexlocofthelabelposition.)

Sets bar index or bar time (depending on the xloc) of the label position.
Syntax
label.set_x(id, x) → void
Arguments
id (series label) Label object.
x (series int) New bar index or bar time of the label position. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
See also
label.new()
label.set_xloc()

## [Sets x-location and new bar index/time value.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsx-locationandnewbarindextimevalue.)

Sets x-location and new bar index/time value.
Syntax
label.set_xloc(id, x, xloc) → void
Arguments
id (series label) Label object.
x (series int) New bar index or bar time of the label position.
xloc (series string) New x-location value.
See also
xloc.bar_index
xloc.bar_time
label.new()
label.set_xy()

## [Sets bar index/time and price of the label position.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsbarindextimeandpriceofthelabelposition.)

Sets bar index/time and price of the label position.
Syntax
label.set_xy(id, x, y) → void
Arguments
id (series label) Label object.
x (series int) New bar index or bar time of the label position. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
y (series int/float) New price of the label position.
See also
label.new()
label.set_y()

## [Sets price of the label position](https://www.tradingview.com/pine-script-reference/v6/#fun_Setspriceofthelabelposition)

Sets price of the label position
Syntax
label.set_y(id, y) → void
Arguments
id (series label) Label object.
y (series int/float) New price of the label position.
See also
label.new()
label.set_yloc()

## [Sets new y-location calculation algorithm.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsnewy-locationcalculationalgorithm.)

Sets new y-location calculation algorithm.
Syntax
label.set_yloc(id, yloc) → void
Arguments
id (series label) Label object.
yloc (series string) New y-location value.
See also
yloc.price
yloc.abovebar
yloc.belowbar
label.new()
library()

## [Declaration statement identifying a script as a library.](https://www.tradingview.com/pine-script-reference/v6/#fun_Declarationstatementidentifyingascriptasalibrary.)

Declaration statement identifying a script as a library.
Syntax
library(title, overlay, dynamic_requests) → void
Arguments
title (const string) The title of the library and its identifier. It cannot contain spaces, special characters or begin with a digit. It is used as the publication's default title, and to uniquely identify the library in the import statement, when another script uses it. It is also used as the script's name on the chart.
overlay (const bool) If true, the script's visuals appear on the main chart pane if the user adds it to the chart directly, or in another script's pane if the user applies it to that script. If false, the script's visuals appear in a separate pane. Changes to the overlay value apply only after the user adds the script to the chart again. Additionally, if the user moves the script to another pane by selecting a "Move to" option in the script's "More" menu, it does not move back to its original pane after any updates to the source code. The default is false. Strategy-specific labels that display entries and exits will be displayed over the main chart regardless of this setting.
dynamic_requests (const bool) Specifies whether the script can dynamically call functions from the request.*() namespace. Dynamic request.*() calls are allowed within the local scopes of conditional structures (e.g., if), loops (e.g., for), and exported functions. Additionally, such calls allow "series" arguments for many of their parameters. Optional. The default is true. See the User Manual's Dynamic requests section for more information.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
// @description Math library
library("num_methods", overlay = true)
// Calculate "sinh()" from the float parameter `x`
export sinh(float x) =>
    (math.exp(x) - math.exp(-x)) / 2.0
plot(sinh(0))
See also
indicator()
strategy()
line()

## [Casts na to line](https://www.tradingview.com/pine-script-reference/v6/#fun_Castsnatoline)

Casts na to line
Syntax
line(x) → series line
Arguments
x (series line) The value to convert to the specified type, usually na.
Returns
The value of the argument after casting to line.
See also
float()
int()
bool()
color()
string()
label()
line.copy()

## [Clones the line object.](https://www.tradingview.com/pine-script-reference/v6/#fun_Clonesthelineobject.)

Clones the line object.
Syntax
line.copy(id) → series line
Arguments
id (series line) Line object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator('Last 100 bars price range', overlay = true)
LOOKBACK = 100
highest = ta.highest(LOOKBACK)
lowest = ta.lowest(LOOKBACK)
if barstate.islastconfirmedhistory
    var lineTop = line.new(bar_index[LOOKBACK], highest, bar_index, highest, color = color.green)
    var lineBottom = line.copy(lineTop)
    line.set_y1(lineBottom, lowest)
    line.set_y2(lineBottom, lowest)
    line.set_color(lineBottom, color.red)
Returns
New line ID object which may be passed to line.setXXX and line.getXXX functions.
See also
line.new()
line.delete()
line.delete()

## [Deletes the specified line object. If it has already been deleted, does nothing.](https://www.tradingview.com/pine-script-reference/v6/#fun_Deletesthespecifiedlineobject.Ifithasalreadybeendeleteddoesnothing.)

Deletes the specified line object. If it has already been deleted, does nothing.
Syntax
line.delete(id) → void
Arguments
id (series line) Line object to delete.
See also
line.new()
line.get_price()

## [Returns the price level of a line at a given bar index.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthepricelevelofalineatagivenbarindex.)

Returns the price level of a line at a given bar index.
Syntax
line.get_price(id, x) → series float
Arguments
id (series line) Line object.
x (series int) Bar index for which price is required.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("GetPrice", overlay=true)
var line l = na
if bar_index == 10
    l := line.new(0, high[5], bar_index, high)
plot(line.get_price(l, bar_index), color=color.green)
Returns
Price value of line 'id' at bar index 'x'.
Remarks
The line is considered to have been created using 'extend=extend.both'.
This function can only be called for lines created using 'xloc.bar_index'. If you try to call it for a line created with 'xloc.bar_time', it will generate an error.
See also
line.new()
line.get_x1()

## [Returns UNIX time or bar index (depending on the last xloc value set) of the first point of the line.](https://www.tradingview.com/pine-script-reference/v6/#fun_ReturnsUNIXtimeorbarindexdependingonthelastxlocvaluesetofthefirstpointoftheline.)

Returns UNIX time or bar index (depending on the last xloc value set) of the first point of the line.
Syntax
line.get_x1(id) → series int
Arguments
id (series line) Line object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("line.get_x1")
my_line = line.new(time, open, time + 60 *60* 24, close, xloc=xloc.bar_time)
a = line.get_x1(my_line)
plot(time - line.get_x1(my_line)) //draws zero plot
Returns
UNIX timestamp (in milliseconds) or bar index.
See also
line.new()
line.get_x2()

## [Returns UNIX time or bar index (depending on the last xloc value set) of the second point of the line.](https://www.tradingview.com/pine-script-reference/v6/#fun_ReturnsUNIXtimeorbarindexdependingonthelastxlocvaluesetofthesecondpointoftheline.)

Returns UNIX time or bar index (depending on the last xloc value set) of the second point of the line.
Syntax
line.get_x2(id) → series int
Arguments
id (series line) Line object.
Returns
UNIX timestamp (in milliseconds) or bar index.
See also
line.new()
line.get_y1()

## [Returns price of the first point of the line.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnspriceofthefirstpointoftheline.)

Returns price of the first point of the line.
Syntax
line.get_y1(id) → series float
Arguments
id (series line) Line object.
Returns
Price value.
See also
line.new()
line.get_y2()

## [Returns price of the second point of the line.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnspriceofthesecondpointoftheline.)

Returns price of the second point of the line.
Syntax
line.get_y2(id) → series float
Arguments
id (series line) Line object.
Returns
Price value.
See also
line.new()
line.new()2 overloads

## [Creates new line object.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createsnewlineobject.)

Creates new line object.
Syntax & Overloads
line.new(first_point, second_point, xloc, extend, color, style, width, force_overlay) → series line
line.new(x1, y1, x2, y2, xloc, extend, color, style, width, force_overlay) → series line
Arguments
first_point (chart.point) A chart.point object that specifies the line's starting coordinate.
second_point (chart.point) A chart.point object that specifies the line's ending coordinate.
xloc (series string) See description of x1 argument. Possible values: xloc.bar_index and xloc.bar_time. Default is xloc.bar_index.
extend (series string) If extend=extend.none, draws segment starting at point (x1, y1) and ending at point (x2, y2). If extend is equal to extend.right or extend.left, draws a ray starting at point (x1, y1) or (x2, y2), respectively. If extend=extend.both, draws a straight line that goes through these points. Default value is extend.none.
color (series color) Line color.
style (series string) Line style. Possible values: line.style_solid, line.style_dotted, line.style_dashed, line.style_arrow_left, line.style_arrow_right, line.style_arrow_both.
width (series int) Line width in pixels.
force_overlay (const bool) If true, the drawing will display on the main chart pane, even when the script occupies a separate pane. Optional. The default is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("line.new")
var line1 = line.new(0, low, bar_index, high, extend=extend.right)
var line2 = line.new(time, open, time + 60 *60* 24, close, xloc=xloc.bar_time, style=line.style_dashed)
line.set_x2(line1, 0)
line.set_xloc(line1, time, time + 60 *60* 24, xloc.bar_time)
line.set_color(line2, color.green)
line.set_width(line2, 5)
Returns
Line ID object which may be passed to line.setXXX and line.getXXX functions.
See also
line.delete()
line.set_x1()
line.set_y1()
line.set_xy1()
line.set_x2()
line.set_y2()
line.set_xy2()
line.set_xloc()
line.set_color()
line.set_extend()
line.set_style()
line.set_width()
line.set_color()

## [Sets the line color](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsthelinecolor)

Sets the line color
Syntax
line.set_color(id, color) → void
Arguments
id (series line) Line object.
color (series color) New line color
See also
line.new()
line.set_extend()

## [Sets extending type of this line object. If extend=extend.none, draws segment starting at point (x1, y1) and ending at point (x2, y2). If extend is equal to extend.right or extend.left, draws a ray starting at point (x1, y1) or (x2, y2), respectively. If extend=extend.both, draws a straight line that goes through these points.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsextendingtypeofthislineobject.Ifextendextend.nonedrawssegmentstartingatpointx1y1andendingatpointx2y2.Ifextendisequaltoextend.rightorextend.leftdrawsaraystartingatpointx1y1orx2y2respectively.Ifextendextend.bothdrawsastraightlinethatgoesthroughthesepoints.)

Sets extending type of this line object. If extend=extend.none, draws segment starting at point (x1, y1) and ending at point (x2, y2). If extend is equal to extend.right or extend.left, draws a ray starting at point (x1, y1) or (x2, y2), respectively. If extend=extend.both, draws a straight line that goes through these points.
Syntax
line.set_extend(id, extend) → void
Arguments
id (series line) Line object.
extend (series string) New extending type.
See also
extend.none
extend.right
extend.left
extend.both
line.new()
line.set_first_point()

## [Sets the first point of the id line to point.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsthefirstpointoftheidlinetopoint.)

Sets the first point of the id line to point.
Syntax
line.set_first_point(id, point) → void
Arguments
id (series line) A line object.
point (chart.point) A chart.point object.
line.set_second_point()

## [Sets the second point of the id line to point.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsthesecondpointoftheidlinetopoint.)

Sets the second point of the id line to point.
Syntax
line.set_second_point(id, point) → void
Arguments
id (series line) A line object.
point (chart.point) A chart.point object.
line.set_style()

## [Sets the line style](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsthelinestyle)

Sets the line style
Syntax
line.set_style(id, style) → void
Arguments
id (series line) Line object.
style (series string) New line style.
See also
line.style_solid
line.style_dotted
line.style_dashed
line.style_arrow_left
line.style_arrow_right
line.style_arrow_both
line.new()
line.set_width()

## [Sets the line width.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsthelinewidth.)

Sets the line width.
Syntax
line.set_width(id, width) → void
Arguments
id (series line) Line object.
width (series int) New line width in pixels.
See also
line.new()
line.set_x1()

## [Sets bar index or bar time (depending on the xloc) of the first point.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsbarindexorbartimedependingonthexlocofthefirstpoint.)

Sets bar index or bar time (depending on the xloc) of the first point.
Syntax
line.set_x1(id, x) → void
Arguments
id (series line) Line object.
x (series int) Bar index or bar time. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
See also
line.new()
line.set_x2()

## [Sets bar index or bar time (depending on the xloc) of the second point.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsbarindexorbartimedependingonthexlocofthesecondpoint.)

Sets bar index or bar time (depending on the xloc) of the second point.
Syntax
line.set_x2(id, x) → void
Arguments
id (series line) Line object.
x (series int) Bar index or bar time. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
See also
line.new()
line.set_xloc()

## [Sets x-location and new bar index/time values.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsx-locationandnewbarindextimevalues.)

Sets x-location and new bar index/time values.
Syntax
line.set_xloc(id, x1, x2, xloc) → void
Arguments
id (series line) Line object.
x1 (series int) Bar index or bar time of the first point.
x2 (series int) Bar index or bar time of the second point.
xloc (series string) New x-location value.
See also
xloc.bar_index
xloc.bar_time
line.new()
line.set_xy1()

## [Sets bar index/time and price of the first point.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsbarindextimeandpriceofthefirstpoint.)

Sets bar index/time and price of the first point.
Syntax
line.set_xy1(id, x, y) → void
Arguments
id (series line) Line object.
x (series int) Bar index or bar time. Note that objects positioned using xloc.bar_index cannot be drawn further than 500 bars into the future.
y (series int/float) Price.
See also
line.new()
line.set_xy2()

## [Sets bar index/time and price of the second point](https://www.tradingview.com/pine-script-reference/v6/#fun_Setsbarindextimeandpriceofthesecondpoint)

Sets bar index/time and price of the second point
Syntax
line.set_xy2(id, x, y) → void
Arguments
id (series line) Line object.
x (series int) Bar index or bar time.
y (series int/float) Price.
See also
line.new()
line.set_y1()

## [Sets price of the first point](https://www.tradingview.com/pine-script-reference/v6/#fun_Setspriceofthefirstpoint)

Sets price of the first point
Syntax
line.set_y1(id, y) → void
Arguments
id (series line) Line object.
y (series int/float) Price.
See also
line.new()
line.set_y2()

## [Sets price of the second point.](https://www.tradingview.com/pine-script-reference/v6/#fun_Setspriceofthesecondpoint.)

Sets price of the second point.
Syntax
line.set_y2(id, y) → void
Arguments
id (series line) Line object.
y (series int/float) Price.
See also
line.new()
linefill()

## [Casts na to linefill.](https://www.tradingview.com/pine-script-reference/v6/#fun_Castsnatolinefill.)

Casts na to linefill.
Syntax
linefill(x) → series linefill
Arguments
x (series linefill) The value to convert to the specified type, usually na.
Returns
The value of the argument after casting to linefill.
See also
float()
int()
bool()
color()
string()
line()
label()
linefill.delete()

## [Deletes the specified linefill object. If it has already been deleted, does nothing.](https://www.tradingview.com/pine-script-reference/v6/#fun_Deletesthespecifiedlinefillobject.Ifithasalreadybeendeleteddoesnothing.)

Deletes the specified linefill object. If it has already been deleted, does nothing.
Syntax
linefill.delete(id) → void
Arguments
id (series linefill) A linefill object.
linefill.get_line1()

## [Returns the ID of the first line used in the id linefill.](https://www.tradingview.com/pine-script-reference/v6/#fun_ReturnstheIDofthefirstlineusedintheidlinefill.)

Returns the ID of the first line used in the id linefill.
Syntax
linefill.get_line1(id) → series line
Arguments
id (series linefill) A linefill object.
linefill.get_line2()

## [Returns the ID of the second line used in the id linefill.](https://www.tradingview.com/pine-script-reference/v6/#fun_ReturnstheIDofthesecondlineusedintheidlinefill.)

Returns the ID of the second line used in the id linefill.
Syntax
linefill.get_line2(id) → series line
Arguments
id (series linefill) A linefill object.
linefill.new()

## [Creates a new linefill object and displays it on the chart, filling the space between line1 and line2 with the color specified in color.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createsanewlinefillobjectanddisplaysitonthechartfillingthespacebetweenline1andline2withthecolorspecifiedincolor.)

Creates a new linefill object and displays it on the chart, filling the space between line1 and line2 with the color specified in color.
Syntax
linefill.new(line1, line2, color) → series linefill
Arguments
line1 (series line) First line object.
line2 (series line) Second line object.
color (series color) The color used to fill the space between the lines.
Returns
The ID of a linefill object that can be passed to other linefill.*() functions.
Remarks
If any line of the two is deleted, the linefill object is also deleted. If the lines are moved (e.g. via line.set_xy() functions), the linefill object is also moved.
If both lines are extended in the same direction relative to the lines themselves (e.g. both have extend.right as the value of their extend= parameter), the space between line extensions will also be filled.
linefill.set_color()

## [The function sets the color of the linefill object passed to it.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionsetsthecolorofthelinefillobjectpassedtoit.)

The function sets the color of the linefill object passed to it.
Syntax
linefill.set_color(id, color) → void
Arguments
id (series linefill) A linefill object.
color (series color) The color of the linefill object.

## [log.error()2 overloads](https://www.tradingview.com/pine-script-reference/v6/#fun_log.error2overloads)

log.error()2 overloads

## [Converts the formatting string and value(s) into a formatted string, and sends the result to the "Pine logs" menu tagged with the "error" debug level.](https://www.tradingview.com/pine-script-reference/v6/#fun_ConvertstheformattingstringandvaluesintoaformattedstringandsendstheresulttothePinelogsmenutaggedwiththeerrordebuglevel.)

Converts the formatting string and value(s) into a formatted string, and sends the result to the "Pine logs" menu tagged with the "error" debug level.
The formatting string can contain literal text and one placeholder in curly braces {} for each value to be formatted. Each placeholder consists of the index of the required argument (beginning at 0) that will replace it, and an optional format specifier. The index represents the position of that argument in the function's argument list.
Syntax & Overloads
log.error(message) → void
log.error(formatString, arg0, arg1, ...) → void
Arguments
message (series string) Log message.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("My strategy", overlay = true, process_orders_on_close = true)
bracketTickSizeInput = input.int(1000, "Stoploss/Take-Profit distance (in ticks)")

## [longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))](https://www.tradingview.com/pine-script-reference/v6/#fun_longConditionta.crossoverta.smaclose14ta.smaclose28)

longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))
if (longCondition)
    limitLevel = close * 1.01
    log.info("Long limit order has been placed at {0}", limitLevel)
    strategy.order("My Long Entry Id", strategy.long, limit = limitLevel)

## [log.info("Exit orders have been placed: Take-profit at {0}, Stop-loss at {1}", close, limitLevel)](https://www.tradingview.com/pine-script-reference/v6/#fun_log.infoExitordershavebeenplacedTake-profitat0Stop-lossat1closelimitLevel)

log.info("Exit orders have been placed: Take-profit at {0}, Stop-loss at {1}", close, limitLevel)
    strategy.exit("Exit", "My Long Entry Id", profit = bracketTickSizeInput, loss = bracketTickSizeInput)

## [if strategy.opentrades > 10](https://www.tradingview.com/pine-script-reference/v6/#fun_ifstrategy.opentrades10)

if strategy.opentrades > 10
    log.warning("{0} positions opened in the same direction in a row. Try adjusting `bracketTickSizeInput`", strategy.opentrades)

## [last10Perc = strategy.initial_capital / 10 > strategy.equity](https://www.tradingview.com/pine-script-reference/v6/#fun_last10Percstrategy.initial_capital10strategy.equity)

last10Perc = strategy.initial_capital / 10 > strategy.equity
if (last10Perc and not last10Perc[1])
    log.error("The strategy has lost 90% of the initial capital!")
Returns
The formatted string.
Remarks
Any curly braces within an unquoted pattern must be balanced. For example, "ab {0} de" and "ab '}' de" are valid patterns, but "ab {0'}' de", "ab } de" and "''{''" are not.
The function can apply additional formatting to some values inside of the {}. The list of additional formatting options can be found in the EXAMPLE section of the str.format() article.
The string used as the formatString argument can contain single quote characters ('). However, one must pair all single quotes in that string to avoid unexpected formatting results.
The "Pine logs..." button is accessible from the "More" dropdown in the Pine Editor and from the "More" dropdown in the status line of any script that uses log.*() functions.
log.info()2 overloads

## [Converts the formatting string and value(s) into a formatted string, and sends the result to the "Pine logs" menu tagged with the "info" debug level.](https://www.tradingview.com/pine-script-reference/v6/#fun_ConvertstheformattingstringandvaluesintoaformattedstringandsendstheresulttothePinelogsmenutaggedwiththeinfodebuglevel.)

Converts the formatting string and value(s) into a formatted string, and sends the result to the "Pine logs" menu tagged with the "info" debug level.
The formatting string can contain literal text and one placeholder in curly braces {} for each value to be formatted. Each placeholder consists of the index of the required argument (beginning at 0) that will replace it, and an optional format specifier. The index represents the position of that argument in the function's argument list.
Syntax & Overloads
log.info(message) → void
log.info(formatString, arg0, arg1, ...) → void
Arguments
message (series string) Log message.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("My strategy", overlay = true, process_orders_on_close = true)
bracketTickSizeInput = input.int(1000, "Stoploss/Take-Profit distance (in ticks)")

## [longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))](https://www.tradingview.com/pine-script-reference/v6/#fun_longConditionta.crossoverta.smaclose14ta.smaclose28)

longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))
if (longCondition)
    limitLevel = close * 1.01
    log.info("Long limit order has been placed at {0}", limitLevel)
    strategy.order("My Long Entry Id", strategy.long, limit = limitLevel)

## [log.info("Exit orders have been placed: Take-profit at {0}, Stop-loss at {1}", close, limitLevel)](https://www.tradingview.com/pine-script-reference/v6/#fun_log.infoExitordershavebeenplacedTake-profitat0Stop-lossat1closelimitLevel)

log.info("Exit orders have been placed: Take-profit at {0}, Stop-loss at {1}", close, limitLevel)
    strategy.exit("Exit", "My Long Entry Id", profit = bracketTickSizeInput, loss = bracketTickSizeInput)

## [if strategy.opentrades > 10](https://www.tradingview.com/pine-script-reference/v6/#fun_ifstrategy.opentrades10)

if strategy.opentrades > 10
    log.warning("{0} positions opened in the same direction in a row. Try adjusting `bracketTickSizeInput`", strategy.opentrades)

## [last10Perc = strategy.initial_capital / 10 > strategy.equity](https://www.tradingview.com/pine-script-reference/v6/#fun_last10Percstrategy.initial_capital10strategy.equity)

last10Perc = strategy.initial_capital / 10 > strategy.equity
if (last10Perc and not last10Perc[1])
    log.error("The strategy has lost 90% of the initial capital!")
Returns
The formatted string.
Remarks
Any curly braces within an unquoted pattern must be balanced. For example, "ab {0} de" and "ab '}' de" are valid patterns, but "ab {0'}' de", "ab } de" and "''{''" are not.
The function can apply additional formatting to some values inside of the {}. The list of additional formatting options can be found in the EXAMPLE section of the str.format() article.
The string used as the formatString argument can contain single quote characters ('). However, one must pair all single quotes in that string to avoid unexpected formatting results.
The "Pine logs..." button is accessible from the "More" dropdown in the Pine Editor and from the "More" dropdown in the status line of any script that uses log.*() functions.
log.warning()2 overloads

## [Converts the formatting string and value(s) into a formatted string, and sends the result to the "Pine logs" menu tagged with the "warning" debug level.](https://www.tradingview.com/pine-script-reference/v6/#fun_ConvertstheformattingstringandvaluesintoaformattedstringandsendstheresulttothePinelogsmenutaggedwiththewarningdebuglevel.)

Converts the formatting string and value(s) into a formatted string, and sends the result to the "Pine logs" menu tagged with the "warning" debug level.
The formatting string can contain literal text and one placeholder in curly braces {} for each value to be formatted. Each placeholder consists of the index of the required argument (beginning at 0) that will replace it, and an optional format specifier. The index represents the position of that argument in the function's argument list.
Syntax & Overloads
log.warning(message) → void
log.warning(formatString, arg0, arg1, ...) → void
Arguments
message (series string) Log message.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("My strategy", overlay = true, process_orders_on_close = true)
bracketTickSizeInput = input.int(1000, "Stoploss/Take-Profit distance (in ticks)")

## [longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))](https://www.tradingview.com/pine-script-reference/v6/#fun_longConditionta.crossoverta.smaclose14ta.smaclose28)

longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))
if (longCondition)
    limitLevel = close * 1.01
    log.info("Long limit order has been placed at {0}", limitLevel)
    strategy.order("My Long Entry Id", strategy.long, limit = limitLevel)

## [log.info("Exit orders have been placed: Take-profit at {0}, Stop-loss at {1}", close, limitLevel)](https://www.tradingview.com/pine-script-reference/v6/#fun_log.infoExitordershavebeenplacedTake-profitat0Stop-lossat1closelimitLevel)

log.info("Exit orders have been placed: Take-profit at {0}, Stop-loss at {1}", close, limitLevel)
    strategy.exit("Exit", "My Long Entry Id", profit = bracketTickSizeInput, loss = bracketTickSizeInput)

## [if strategy.opentrades > 10](https://www.tradingview.com/pine-script-reference/v6/#fun_ifstrategy.opentrades10)

if strategy.opentrades > 10
    log.warning("{0} positions opened in the same direction in a row. Try adjusting `bracketTickSizeInput`", strategy.opentrades)

## [last10Perc = strategy.initial_capital / 10 > strategy.equity](https://www.tradingview.com/pine-script-reference/v6/#fun_last10Percstrategy.initial_capital10strategy.equity)

last10Perc = strategy.initial_capital / 10 > strategy.equity
if (last10Perc and not last10Perc[1])
    log.error("The strategy has lost 90% of the initial capital!")
Returns
The formatted string.
Remarks
Any curly braces within an unquoted pattern must be balanced. For example, "ab {0} de" and "ab '}' de" are valid patterns, but "ab {0'}' de", "ab } de" and "''{''" are not.
The function can apply additional formatting to some values inside of the {}. The list of additional formatting options can be found in the EXAMPLE section of the str.format() article.
The string used as the formatString argument can contain single quote characters ('). However, one must pair all single quotes in that string to avoid unexpected formatting results.
The "Pine logs..." button is accessible from the "More" dropdown in the Pine Editor and from the "More" dropdown in the status line of any script that uses log.*() functions.
map.clear()

## [Clears the map, removing all key-value pairs from it.](https://www.tradingview.com/pine-script-reference/v6/#fun_Clearsthemapremovingallkey-valuepairsfromit.)

Clears the map, removing all key-value pairs from it.
Syntax
map.clear(id) → void
Arguments
id (any map type) A map object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("map.clear example")
oddMap = map.new<int, bool>()
oddMap.put(1, true)
oddMap.put(2, false)
oddMap.put(3, true)
map.clear(oddMap)
plot(oddMap.size())
See also
map.new<type,type>()
map.put_all()
map.keys()
map.values()
map.remove()
map.contains()

## [Returns true if the key was found in the id map, false otherwise.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnstrueifthekeywasfoundintheidmapfalseotherwise.)

Returns true if the key was found in the id map, false otherwise.
Syntax
map.contains(id, key) → series bool
Arguments
id (any map type) A map object.
key (series <type of the map's elements>) The key to search in the map.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("map.includes example")
a = map.new<string, float>()
a.put("open", open)
p = close
if map.contains(a, "open")
    p := a.get("open")
plot(p)
See also
map.new<type,type>()
map.put()
map.keys()
map.values()
map.size()
map.copy()

## [Creates a copy of an existing map.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createsacopyofanexistingmap.)

Creates a copy of an existing map.
Syntax
map.copy(id) → map<keyType, valueType>
Arguments
id (any map type) A map object to copy.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("map.copy example")
a = map.new<string, int>()
a.put("example", 1)
b = map.copy(a)
a := map.new<string, int>()
a.put("example", 2)
plot(a.get("example"))
plot(b.get("example"))
Returns
A copy of the id map.
See also
map.new<type,type>()
map.put()
map.keys()
map.values()
map.get()
map.size()
map.get()

## [Returns the value associated with the specified key in the id map.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthevalueassociatedwiththespecifiedkeyintheidmap.)

Returns the value associated with the specified key in the id map.
Syntax
map.get(id, key) → <value_type>
Arguments
id (any map type) A map object.
key (series <type of the map's elements>) The key of the value to retrieve.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("map.get example")
a = map.new<int, int>()
size = 10
for i = 0 to size
    a.put(i, size-i)
plot(map.get(a, 1))
See also
map.new<type,type>()
map.put()
map.keys()
map.values()
map.contains()
map.keys()

## [Returns an array of all the keys in the id map. The resulting array is a copy and any changes to it are not reflected in the original map.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsanarrayofallthekeysintheidmap.Theresultingarrayisacopyandanychangestoitarenotreflectedintheoriginalmap.)

Returns an array of all the keys in the id map. The resulting array is a copy and any changes to it are not reflected in the original map.
Syntax
map.keys(id) → array<type>
Arguments
id (any map type) A map object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("map.keys example")
a = map.new<string, float>()
a.put("open", open)
a.put("high", high)
a.put("low", low)
a.put("close", close)
keys = map.keys(a)
ohlc = 0.0
for key in keys
    ohlc += a.get(key)
plot(ohlc/4)
Remarks
Maps maintain insertion order. The elements within the array returned by this function will also be in the insertion order.
See also
map.new<type,type>()
map.put()
map.get()
map.values()
map.size()
map.new<type,type>()

## [Creates a new map object: a collection that consists of key-value pairs, where all keys are of the keyType, and all values are of the valueType.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createsanewmapobjectacollectionthatconsistsofkey-valuepairswhereallkeysareofthekeyTypeandallvaluesareofthevalueType.)

Creates a new map object: a collection that consists of key-value pairs, where all keys are of the keyType, and all values are of the valueType.
keyType can be a primitive type or enum. For example: int, float, bool, string, color.
valueType can be of any type except array<>, matrix<>, and map<>. User-defined types are allowed, even if they have array<>, matrix<>, or map<> as one of their fields.
Syntax
map.new<keyType, valueType>() → map<keyType, valueType>
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("map.new<string, int> example")
a = map.new<string, int>()
a.put("example", 1)
label.new(bar_index, close, str.tostring(a.get("example")))
Returns
The ID of a map object which may be used in other map.*() functions.
Remarks
Each key is unique and can only appear once. When adding a new value with a key that the map already contains, that value replaces the old value associated with the key.
Maps maintain insertion order. Note that the order does not change when inserting a pair with a key that's already in the map. The new pair replaces the existing pair with the key in such cases.
See also
map.put()
map.keys()
map.values()
map.get()
array.new<type>()
map.put()

## [Puts a new key-value pair into the id map.](https://www.tradingview.com/pine-script-reference/v6/#fun_Putsanewkey-valuepairintotheidmap.)

Puts a new key-value pair into the id map.
Syntax
map.put(id, key, value) → <value_type>
Arguments
id (any map type) A map object.
key (series <type of the map's elements>) The key to put into the map.
value (series <type of the map's elements>) The key value to put into the map.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("map.put example")
a = map.new<string, float>()
map.put(a, "first", 10)
map.put(a, "second", 15)
prevFirst = map.put(a, "first", 20)
currFirst = a.get("first")
plot(prevFirst)
plot(currFirst)
Returns
The previous value associated with key if the key was already present in the map, or na if the key is new.
Remarks
Maps maintain insertion order. Note that the order does not change when inserting a pair with a key that's already in the map. The new pair replaces the existing pair with the key in such cases.
See also
map.new<type,type>()
map.put_all()
map.keys()
map.values()
map.remove()
map.put_all()

## [Puts all key-value pairs from the id2 map into the id map.](https://www.tradingview.com/pine-script-reference/v6/#fun_Putsallkey-valuepairsfromtheid2mapintotheidmap.)

Puts all key-value pairs from the id2 map into the id map.
Syntax
map.put_all(id, id2) → void
Arguments
id (any map type) A map object to append to.
id2 (any map type) A map object to be appended.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("map.put_all example")
a = map.new<string, float>()
b = map.new<string, float>()
a.put("first", 10)
a.put("second", 15)
b.put("third", 20)
map.put_all(a, b)
plot(a.get("third"))
See also
map.new<type,type>()
map.put()
map.keys()
map.values()
map.remove()
map.remove()

## [Removes a key-value pair from the id map.](https://www.tradingview.com/pine-script-reference/v6/#fun_Removesakey-valuepairfromtheidmap.)

Removes a key-value pair from the id map.
Syntax
map.remove(id, key) → <value_type>
Arguments
id (any map type) A map object.
key (series <type of the map's elements>) The key of the pair to remove from the map.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("map.remove example")
a = map.new<string, color>()
a.put("firstColor", color.green)
oldColorValue = map.remove(a, "firstColor")
plot(close, color = oldColorValue)
Returns
The previous value associated with key if the key was present in the map, or na if there was no such key.
See also
map.new<type,type>()
map.put()
map.keys()
map.values()
map.clear()
map.size()

## [Returns the number of key-value pairs in the id map.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthenumberofkey-valuepairsintheidmap.)

Returns the number of key-value pairs in the id map.
Syntax
map.size(id) → series int
Arguments
id (any map type) A map object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("map.size example")
a = map.new<int, int>()
size = 10
for i = 0 to size
    a.put(i, size-i)
plot(map.size(a))
See also
map.new<type,type>()
map.put()
map.keys()
map.values()
map.get()
map.values()

## [Returns an array of all the values in the id map. The resulting array is a copy and any changes to it are not reflected in the original map.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsanarrayofallthevaluesintheidmap.Theresultingarrayisacopyandanychangestoitarenotreflectedintheoriginalmap.)

Returns an array of all the values in the id map. The resulting array is a copy and any changes to it are not reflected in the original map.
Syntax
map.values(id) → array<type>
Arguments
id (any map type) A map object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("map.values example")
a = map.new<string, float>()
a.put("open", open)
a.put("high", high)
a.put("low", low)
a.put("close", close)
values = map.values(a)
ohlc = 0.0
for value in values
    ohlc += value
plot(ohlc/4)
Remarks
Maps maintain insertion order. The elements within the array returned by this function will also be in the insertion order.
See also
map.new<type,type>()
map.put()
map.get()
map.keys()
map.size()
math.abs()8 overloads

## [Absolute value of number is number if number >= 0, or -number otherwise.](https://www.tradingview.com/pine-script-reference/v6/#fun_Absolutevalueofnumberisnumberifnumber0or-numberotherwise.)

Absolute value of number is number if number >= 0, or -number otherwise.
Syntax & Overloads
math.abs(number) → const int
math.abs(number) → input int
math.abs(number) → const float
math.abs(number) → simple int
math.abs(number) → input float
math.abs(number) → series int
math.abs(number) → simple float
math.abs(number) → series float
Arguments
number (const int) The number to use in the calculation.
Returns
The absolute value of number.
math.acos()4 overloads

## [The acos function returns the arccosine (in radians) of number such that cos(acos(y)) = y for y in range [-1, 1].](https://www.tradingview.com/pine-script-reference/v6/#fun_Theacosfunctionreturnsthearccosineinradiansofnumbersuchthatcosacosyyforyinrange-11.)

The acos function returns the arccosine (in radians) of number such that cos(acos(y)) = y for y in range [-1, 1].
Syntax & Overloads
math.acos(angle) → const float
math.acos(angle) → input float
math.acos(angle) → simple float
math.acos(angle) → series float
Arguments
angle (const int/float) The value, in radians, to use in the calculation.
Returns
The arc cosine of a value; the returned angle is in the range [0, Pi], or na if y is outside of range [-1, 1].
math.asin()4 overloads

## [The asin function returns the arcsine (in radians) of number such that sin(asin(y)) = y for y in range [-1, 1].](https://www.tradingview.com/pine-script-reference/v6/#fun_Theasinfunctionreturnsthearcsineinradiansofnumbersuchthatsinasinyyforyinrange-11.)

The asin function returns the arcsine (in radians) of number such that sin(asin(y)) = y for y in range [-1, 1].
Syntax & Overloads
math.asin(angle) → const float
math.asin(angle) → input float
math.asin(angle) → simple float
math.asin(angle) → series float
Arguments
angle (const int/float) The value, in radians, to use in the calculation.
Returns
The arcsine of a value; the returned angle is in the range [-Pi/2, Pi/2], or na if y is outside of range [-1, 1].
math.atan()4 overloads

## [The atan function returns the arctangent (in radians) of number such that tan(atan(y)) = y for any y.](https://www.tradingview.com/pine-script-reference/v6/#fun_Theatanfunctionreturnsthearctangentinradiansofnumbersuchthattanatanyyforanyy.)

The atan function returns the arctangent (in radians) of number such that tan(atan(y)) = y for any y.
Syntax & Overloads
math.atan(angle) → const float
math.atan(angle) → input float
math.atan(angle) → simple float
math.atan(angle) → series float
Arguments
angle (const int/float) The value, in radians, to use in the calculation.
Returns
The arc tangent of a value; the returned angle is in the range [-Pi/2, Pi/2].
math.avg()2 overloads

## [Calculates average of all given series (elementwise).](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculatesaverageofallgivenserieselementwise.)

Calculates average of all given series (elementwise).
Syntax & Overloads
math.avg(number0, number1, ...) → simple float
math.avg(number0, number1, ...) → series float
Arguments
number0, number1, ... (simple int/float) A sequence of numbers to use in the calculation.
Returns
Average.
See also
math.sum()
ta.cum()
ta.sma()
math.ceil()4 overloads

## [Rounds the specified number up to the smallest whole number ("int" value) that is greater than or equal to it.](https://www.tradingview.com/pine-script-reference/v6/#fun_Roundsthespecifiednumberuptothesmallestwholenumberintvaluethatisgreaterthanorequaltoit.)

Rounds the specified number up to the smallest whole number ("int" value) that is greater than or equal to it.
Syntax & Overloads
math.ceil(number) → const int
math.ceil(number) → input int
math.ceil(number) → simple int
math.ceil(number) → series int
Arguments
number (const int/float) The number to round.
Returns
The smallest "int" value that is greater than or equal to the number.
See also
math.floor()
math.round()
math.cos()4 overloads

## [The cos function returns the trigonometric cosine of an angle.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thecosfunctionreturnsthetrigonometriccosineofanangle.)

The cos function returns the trigonometric cosine of an angle.
Syntax & Overloads
math.cos(angle) → const float
math.cos(angle) → input float
math.cos(angle) → simple float
math.cos(angle) → series float
Arguments
angle (const int/float) Angle, in radians.
Returns
The trigonometric cosine of an angle.
math.exp()4 overloads

## [The exp function of number is e raised to the power of number, where e is Euler's number.](https://www.tradingview.com/pine-script-reference/v6/#fun_TheexpfunctionofnumberiseraisedtothepowerofnumberwhereeisEulersnumber.)

The exp function of number is e raised to the power of number, where e is Euler's number.
Syntax & Overloads
math.exp(number) → const float
math.exp(number) → input float
math.exp(number) → simple float
math.exp(number) → series float
Arguments
number (const int/float) The number to use in the calculation.
Returns
A value representing e raised to the power of number.
See also
math.pow()
math.floor()4 overloads

## [Rounds the specified number down to the largest whole number ("int" value) that is less than or equal to it.](https://www.tradingview.com/pine-script-reference/v6/#fun_Roundsthespecifiednumberdowntothelargestwholenumberintvaluethatislessthanorequaltoit.)

Rounds the specified number down to the largest whole number ("int" value) that is less than or equal to it.
Syntax & Overloads
math.floor(number) → const int
math.floor(number) → input int
math.floor(number) → simple int
math.floor(number) → series int
Arguments
number (const int/float) The number to round.
Returns
The largest "int" value that is less than or equal to the number.
See also
math.ceil()
math.round()
math.log()4 overloads

## [Natural logarithm of any number > 0 is the unique y such that e^y = number.](https://www.tradingview.com/pine-script-reference/v6/#fun_Naturallogarithmofanynumber0istheuniqueysuchthateynumber.)

Natural logarithm of any number > 0 is the unique y such that e^y = number.
Syntax & Overloads
math.log(number) → const float
math.log(number) → input float
math.log(number) → simple float
math.log(number) → series float
Arguments
number (const int/float) The number to use in the calculation.
Returns
The natural logarithm of number.
See also
math.log10()
math.log10()4 overloads

## [The common (or base 10) logarithm of number is the power to which 10 must be raised to obtain the number. 10^y = number.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thecommonorbase10logarithmofnumberisthepowertowhich10mustberaisedtoobtainthenumber.10ynumber.)

The common (or base 10) logarithm of number is the power to which 10 must be raised to obtain the number. 10^y = number.
Syntax & Overloads
math.log10(number) → const float
math.log10(number) → input float
math.log10(number) → simple float
math.log10(number) → series float
Arguments
number (const int/float) The number to use in the calculation.
Returns
The base 10 logarithm of number.
See also
math.log()
math.max()8 overloads

## [Returns the greatest of multiple values.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthegreatestofmultiplevalues.)

Returns the greatest of multiple values.
Syntax & Overloads
math.max(number0, number1, ...) → const int
math.max(number0, number1, ...) → const float
math.max(number0, number1, ...) → input int
math.max(number0, number1, ...) → simple int
math.max(number0, number1, ...) → input float
math.max(number0, number1, ...) → series int
math.max(number0, number1, ...) → simple float
math.max(number0, number1, ...) → series float
Arguments
number0, number1, ... (const int) A sequence of numbers to use in the calculation.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("math.max", overlay=true)
plot(math.max(close, open))
plot(math.max(close, math.max(open, 42)))
Returns
The greatest of multiple given values.
See also
math.min()
math.min()8 overloads

## [Returns the smallest of multiple values.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthesmallestofmultiplevalues.)

Returns the smallest of multiple values.
Syntax & Overloads
math.min(number0, number1, ...) → const int
math.min(number0, number1, ...) → const float
math.min(number0, number1, ...) → input int
math.min(number0, number1, ...) → simple int
math.min(number0, number1, ...) → input float
math.min(number0, number1, ...) → series int
math.min(number0, number1, ...) → simple float
math.min(number0, number1, ...) → series float
Arguments
number0, number1, ... (const int) A sequence of numbers to use in the calculation.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("math.min", overlay=true)
plot(math.min(close, open))
plot(math.min(close, math.min(open, 42)))
Returns
The smallest of multiple given values.
See also
math.max()
math.pow()4 overloads

## [Mathematical power function.](https://www.tradingview.com/pine-script-reference/v6/#fun_Mathematicalpowerfunction.)

Mathematical power function.
Syntax & Overloads
math.pow(base, exponent) → const float
math.pow(base, exponent) → input float
math.pow(base, exponent) → simple float
math.pow(base, exponent) → series float
Arguments
base (const int/float) Specify the base to use.
exponent (const int/float) Specifies the exponent.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("math.pow", overlay=true)
plot(math.pow(close, 2))
Returns
base raised to the power of exponent. If base is a series, it is calculated elementwise.
See also
math.sqrt()
math.exp()
math.random()

## [Returns a pseudo-random value. The function will generate a different sequence of values for each script execution. Using the same value for the optional seed argument will produce a repeatable sequence.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsapseudo-randomvalue.Thefunctionwillgenerateadifferentsequenceofvaluesforeachscriptexecution.Usingthesamevaluefortheoptionalseedargumentwillproducearepeatablesequence.)

Returns a pseudo-random value. The function will generate a different sequence of values for each script execution. Using the same value for the optional seed argument will produce a repeatable sequence.
Syntax
math.random(min, max, seed) → series float
Arguments
min (series int/float) The lower bound of the range of random values. The value is not included in the range. The default is 0.
max (series int/float) The upper bound of the range of random values. The value is not included in the range. The default is 1.
seed (series int) Optional argument. When the same seed is used, allows successive calls to the function to produce a repeatable set of values.
Returns
A random value.
math.round()8 overloads

## [Returns the value of number rounded to the nearest integer, with ties rounding up. If the precision parameter is used, returns a float value rounded to that amount of decimal places.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthevalueofnumberroundedtothenearestintegerwithtiesroundingup.Iftheprecisionparameterisusedreturnsafloatvalueroundedtothatamountofdecimalplaces.)

Returns the value of number rounded to the nearest integer, with ties rounding up. If the precision parameter is used, returns a float value rounded to that amount of decimal places.
Syntax & Overloads
math.round(number) → const int
math.round(number) → input int
math.round(number) → simple int
math.round(number) → series int
math.round(number, precision) → const float
math.round(number, precision) → input float
math.round(number, precision) → simple float
math.round(number, precision) → series float
Arguments
number (const int/float) The value to be rounded.
Returns
The value of number rounded to the nearest integer, or according to precision.
Remarks
Note that for 'na' values function returns 'na'.
See also
math.ceil()
math.floor()
math.round_to_mintick()2 overloads

## [Returns the value rounded to the symbol's mintick, i.e. the nearest value that can be divided by syminfo.mintick, without the remainder, with ties rounding up.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthevalueroundedtothesymbolsminticki.e.thenearestvaluethatcanbedividedbysyminfo.mintickwithouttheremainderwithtiesroundingup.)

Returns the value rounded to the symbol's mintick, i.e. the nearest value that can be divided by syminfo.mintick, without the remainder, with ties rounding up.
Syntax & Overloads
math.round_to_mintick(number) → simple float
math.round_to_mintick(number) → series float
Arguments
number (simple int/float) The value to be rounded.
Returns
The number rounded to tick precision.
Remarks
Note that for 'na' values function returns 'na'.
See also
math.ceil()
math.floor()
math.sign()4 overloads

## [Sign (signum) of number is zero if number is zero, 1.0 if number is greater than zero, -1.0 if number is less than zero.](https://www.tradingview.com/pine-script-reference/v6/#fun_Signsignumofnumberiszeroifnumberiszero1.0ifnumberisgreaterthanzero-1.0ifnumberislessthanzero.)

Sign (signum) of number is zero if number is zero, 1.0 if number is greater than zero, -1.0 if number is less than zero.
Syntax & Overloads
math.sign(number) → const float
math.sign(number) → input float
math.sign(number) → simple float
math.sign(number) → series float
Arguments
number (const int/float) The number to use in the calculation.
Returns
The sign of the argument.
math.sin()4 overloads

## [The sin function returns the trigonometric sine of an angle.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thesinfunctionreturnsthetrigonometricsineofanangle.)

The sin function returns the trigonometric sine of an angle.
Syntax & Overloads
math.sin(angle) → const float
math.sin(angle) → input float
math.sin(angle) → simple float
math.sin(angle) → series float
Arguments
angle (const int/float) Angle, in radians.
Returns
The trigonometric sine of an angle.
math.sqrt()4 overloads

## [Square root of any number >= 0 is the unique y >= 0 such that y^2 = number.](https://www.tradingview.com/pine-script-reference/v6/#fun_Squarerootofanynumber0istheuniquey0suchthaty2number.)

Square root of any number >= 0 is the unique y >= 0 such that y^2 = number.
Syntax & Overloads
math.sqrt(number) → const float
math.sqrt(number) → input float
math.sqrt(number) → simple float
math.sqrt(number) → series float
Arguments
number (const int/float) The number to use in the calculation.
Returns
The square root of number.
See also
math.pow()
math.sum()

## [The sum function returns the sliding sum of last y values of x.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thesumfunctionreturnstheslidingsumoflastyvaluesofx.)

The sum function returns the sliding sum of last y values of x.
Syntax
math.sum(source, length) → series float
Arguments
source (series int/float) Series of values to process.
length (series int) Number of bars (length).
Returns
Sum of source for length bars back.
Remarks
na values in the source series are ignored; the function calculates on the length quantity of non-na values.
See also
ta.cum()
for
math.tan()4 overloads

## [The tan function returns the trigonometric tangent of an angle.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thetanfunctionreturnsthetrigonometrictangentofanangle.)

The tan function returns the trigonometric tangent of an angle.
Syntax & Overloads
math.tan(angle) → const float
math.tan(angle) → input float
math.tan(angle) → simple float
math.tan(angle) → series float
Arguments
angle (const int/float) Angle, in radians.
Returns
The trigonometric tangent of an angle.
math.todegrees()

## [Returns an approximately equivalent angle in degrees from an angle measured in radians.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsanapproximatelyequivalentangleindegreesfromananglemeasuredinradians.)

Returns an approximately equivalent angle in degrees from an angle measured in radians.
Syntax
math.todegrees(radians) → series float
Arguments
radians (series int/float) Angle in radians.
Returns
The angle value in degrees.
math.toradians()

## [Returns an approximately equivalent angle in radians from an angle measured in degrees.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsanapproximatelyequivalentangleinradiansfromananglemeasuredindegrees.)

Returns an approximately equivalent angle in radians from an angle measured in degrees.
Syntax
math.toradians(degrees) → series float
Arguments
degrees (series int/float) Angle in degrees.
Returns
The angle value in radians.
matrix.add_col()

## [Inserts a new column at the column index of the id matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Insertsanewcolumnatthecolumnindexoftheidmatrix.)

Inserts a new column at the column index of the id matrix.
Syntax
matrix.add_col(id, column, array_id) → void
Arguments
id (any matrix type) The matrix object's ID (reference).
column (series int) Optional. The index of the new column. Must be a value from 0 to matrix.columns(id). All existing columns with indices that are greater than or equal to this value increase their index by one. The default is matrix.columns(id).
array_id (any array type) Optional. The ID of an array to use as the new column. If the matrix is empty, the array can be of any size. Otherwise, its size must equal matrix.rows(id). By default, the function inserts a column of na values.
Adding a column to the matrix
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.add_col()` Example 1")

## [// Create a 2x3 "int" matrix containing values `0`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x3intmatrixcontainingvalues0.)

// Create a 2x3 "int" matrix containing values `0`.
m = matrix.new<int>(2, 3, 0)

## [// Add a column with `na` values to the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Addacolumnwithnavaluestothematrix.)

// Add a column with `na` values to the matrix.
matrix.add_col(m)

## [// Display matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displaymatrixelements.)

// Display matrix elements.
if barstate.islastconfirmedhistory
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m))
Adding an array as a column to the matrix
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.add_col()` Example 2")

## [if barstate.islastconfirmedhistory](https://www.tradingview.com/pine-script-reference/v6/#fun_ifbarstate.islastconfirmedhistory)

if barstate.islastconfirmedhistory
    // Create an empty matrix object.
    var m = matrix.new<int>()

## [// Create an array with values `1` and `3`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createanarraywithvalues1and3.)

// Create an array with values `1` and `3`.
    var a = array.from(1, 3)

## [// Add the `a` array as the first column of the empty matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Addtheaarrayasthefirstcolumnoftheemptymatrix.)

// Add the `a` array as the first column of the empty matrix.
    matrix.add_col(m, 0, a)

## [// Display matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displaymatrixelements.)

// Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m))
Remarks
Rather than add columns to an empty matrix, it is far more efficient to declare a matrix with explicit dimensions and fill it with values. Adding a column is also much slower than adding a row with the matrix.add_row() function.
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.columns()
matrix.rows()
matrix.add_row()
matrix.add_row()

## [Inserts a new row at the row index of the id matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Insertsanewrowattherowindexoftheidmatrix.)

Inserts a new row at the row index of the id matrix.
Syntax
matrix.add_row(id, row, array_id) → void
Arguments
id (any matrix type) The matrix object's ID (reference).
row (series int) Optional. The index of the new row. Must be a value from 0 to matrix.rows(id). All existing rows with indices that are greater than or equal to this value increase their index by one. The default is matrix.rows(id).
array_id (any array type) Optional. The ID of an array to use as the new row. If the matrix is empty, the array can be of any size. Otherwise, its size must equal matrix.columns(id). By default, the function inserts a row of na values.
Adding a row to the matrix
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.add_row()` Example 1")

## [// Create a 2x3 "int" matrix containing values `0`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x3intmatrixcontainingvalues0.)

// Create a 2x3 "int" matrix containing values `0`.
m = matrix.new<int>(2, 3, 0)

## [// Add a row with `na` values to the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Addarowwithnavaluestothematrix.)

// Add a row with `na` values to the matrix.
matrix.add_row(m)

## [// Display matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displaymatrixelements.)

// Display matrix elements.
if barstate.islastconfirmedhistory
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m))
Adding an array as a row to the matrix
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.add_row()` Example 2")

## [if barstate.islastconfirmedhistory](https://www.tradingview.com/pine-script-reference/v6/#fun_ifbarstate.islastconfirmedhistory)

if barstate.islastconfirmedhistory
    // Create an empty matrix object.
    var m = matrix.new<int>()

## [// Create an array with values `1` and `2`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createanarraywithvalues1and2.)

// Create an array with values `1` and `2`.
    var a = array.from(1, 2)

## [// Add the `a` array as the first row of the empty matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Addtheaarrayasthefirstrowoftheemptymatrix.)

// Add the `a` array as the first row of the empty matrix.
    matrix.add_row(m, 0, a)

## [// Display matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displaymatrixelements.)

// Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m))
Remarks
Indexing of rows and columns starts at zero. Rather than add rows to an empty matrix, it is far more efficient to declare a matrix with explicit dimensions and fill it with values.
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.columns()
matrix.rows()
matrix.add_col()
matrix.avg()2 overloads

## [The function calculates the average of all elements in the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncalculatestheaverageofallelementsinthematrix.)

The function calculates the average of all elements in the matrix.
Syntax & Overloads
matrix.avg(id) → series float
matrix.avg(id) → series int
Arguments
id (matrix<int/float>) A matrix object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.avg()` Example")

## [// Create a 2x2 matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x2matrix.)

// Create a 2x2 matrix.
var m = matrix.new<int>(2, 2, na)
// Fill the matrix with values.
matrix.set(m, 0, 0, 1)
matrix.set(m, 0, 1, 2)
matrix.set(m, 1, 0, 3)
matrix.set(m, 1, 1, 4)

## [// Get the average value of the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Gettheaveragevalueofthematrix.)

// Get the average value of the matrix.
var x = matrix.avg(m)

## [plot(x, 'Matrix average value')](https://www.tradingview.com/pine-script-reference/v6/#fun_plotxMatrixaveragevalue)

plot(x, 'Matrix average value')
Returns
The average value from the id matrix.
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.columns()
matrix.rows()
matrix.col()

## [The function creates a one-dimensional array from the elements of a matrix column.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncreatesaone-dimensionalarrayfromtheelementsofamatrixcolumn.)

The function creates a one-dimensional array from the elements of a matrix column.
Syntax
matrix.col(id, column) → array<type>
Arguments
id (any matrix type) A matrix object.
column (series int) Index of the required column.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.col()` Example", "", true)

## [// Create a 2x3 "float" matrix from `hlc3` values.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x3floatmatrixfromhlc3values.)

// Create a 2x3 "float" matrix from `hlc3` values.
m = matrix.new<float>(2, 3, hlc3)

## [// Return an array with the values of the first column of matrix `m`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnanarraywiththevaluesofthefirstcolumnofmatrixm.)

// Return an array with the values of the first column of matrix `m`.
a = matrix.col(m, 0)

## [// Plot the first value from the array `a`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Plotthefirstvaluefromthearraya.)

// Plot the first value from the array `a`.
plot(array.get(a, 0))
Returns
An array ID containing the column values of the id matrix.
Remarks
Indexing of rows starts at 0.
See also
matrix.new<type>()
matrix.get()
array.get()
matrix.col()
matrix.columns()
matrix.columns()

## [The function returns the number of columns in the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthenumberofcolumnsinthematrix.)

The function returns the number of columns in the matrix.
Syntax
matrix.columns(id) → series int
Arguments
id (any matrix type) A matrix object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.columns()` Example")

## [// Create a 2x6 matrix with values `0`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x6matrixwithvalues0.)

// Create a 2x6 matrix with values `0`.
var m = matrix.new<int>(2, 6, 0)

## [// Get the quantity of columns in matrix `m`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Getthequantityofcolumnsinmatrixm.)

// Get the quantity of columns in matrix `m`.
var x = matrix.columns(m)

## [// Display using a label.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingalabel.)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, "Columns: " + str.tostring(x) + "\n" + str.tostring(m))
Returns
The number of columns in the matrix id.
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.col()
matrix.row()
matrix.rows()
matrix.concat()

## [The function appends the m2 matrix to the m1 matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionappendsthem2matrixtothem1matrix.)

The function appends the m2 matrix to the m1 matrix.
Syntax
matrix.concat(id1, id2) → matrix<type>
Arguments
id1 (any matrix type) Matrix object to concatenate into.
id2 (any matrix type) Matrix object whose elements will be appended to id1.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.concat()` Example")

## [// Create a 2x4 "int" matrix containing values `0`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x4intmatrixcontainingvalues0.)

// Create a 2x4 "int" matrix containing values `0`.
m1 = matrix.new<int>(2, 4, 0)
// Create a 2x4 "int" matrix containing values `1`.
m2 = matrix.new<int>(2, 4, 1)

## [// Append matrix `m2` to `m1`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Appendmatrixm2tom1.)

// Append matrix `m2` to `m1`.
matrix.concat(m1, m2)

## [// Display matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displaymatrixelements.)

// Display matrix elements.
if barstate.islastconfirmedhistory
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix Elements:")
    table.cell(t, 0, 1, str.tostring(m1))
Returns
Returns the id1 matrix concatenated with the id2 matrix.
Remarks
The number of columns in both matrices must be identical.
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.columns()
matrix.rows()
matrix.copy()

## [The function creates a new matrix which is a copy of the original.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncreatesanewmatrixwhichisacopyoftheoriginal.)

The function creates a new matrix which is a copy of the original.
Syntax
matrix.copy(id) → matrix<type>
Arguments
id (any matrix type) A matrix object to copy.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.copy()` Example")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 "float" matrix with `1` values.
    var m1 = matrix.new<float>(2, 3, 1)

## [// Copy the matrix to a new one.](https://www.tradingview.com/pine-script-reference/v6/#fun_Copythematrixtoanewone.)

// Copy the matrix to a new one.
    // Note that unlike what `matrix.copy()` does,
    // the simple assignment operation `m2 = m1`
    // would NOT create a new copy of the `m1` matrix.
    // It would merely create a copy of its ID referencing the same matrix.
    var m2 = matrix.copy(m1)

## [// Display using a table.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingatable.)

// Display using a table.
    var t = table.new(position.top_right, 5, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Matrix Copy:")
    table.cell(t, 1, 1, str.tostring(m2))
Returns
A new matrix object of the copied id matrix.
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.columns()
matrix.rows()
matrix.det()2 overloads

## [The function returns the determinant of a square matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthedeterminantofasquarematrix.)

The function returns the determinant of a square matrix.
Syntax & Overloads
matrix.det(id) → series float
matrix.det(id) → series int
Arguments
id (matrix<int/float>) A matrix object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.det` Example")

## [// Create a 2x2 matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x2matrix.)

// Create a 2x2 matrix.
var m = matrix.new<float>(2, 2, na)
// Fill the matrix with values.
matrix.set(m, 0, 0,  3)
matrix.set(m, 0, 1,  7)
matrix.set(m, 1, 0,  1)
matrix.set(m, 1, 1, -4)

## [// Get the determinant of the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Getthedeterminantofthematrix.)

// Get the determinant of the matrix.
var x = matrix.det(m)

## [plot(x, 'Matrix determinant')](https://www.tradingview.com/pine-script-reference/v6/#fun_plotxMatrixdeterminant)

plot(x, 'Matrix determinant')
Returns
The determinant value of the id matrix.
Remarks
Function calculation based on the LU decomposition algorithm.
See also
matrix.new<type>()
matrix.set()
matrix.is_square()
matrix.diff()2 overloads

## [The function returns a new matrix resulting from the subtraction between matrices id1 and id2, or of matrix id1 and an id2 scalar (a numerical value).](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsanewmatrixresultingfromthesubtractionbetweenmatricesid1andid2orofmatrixid1andanid2scalaranumericalvalue.)

The function returns a new matrix resulting from the subtraction between matrices id1 and id2, or of matrix id1 and an id2 scalar (a numerical value).
Syntax & Overloads
matrix.diff(id1, id2) → matrix<int>
matrix.diff(id1, id2) → matrix<float>
Arguments
id1 (matrix<int>) Matrix to subtract from.
id2 (series int/float/matrix<int>) Matrix object or a scalar value to be subtracted.
Difference between two matrices
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.diff()` Example 1")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix containing values `5`.
    var m1 = matrix.new<float>(2, 3, 5)
    // Create a 2x3 matrix containing values `4`.
    var m2 = matrix.new<float>(2, 3, 4)
    // Create a new matrix containing the difference between matrices `m1` and `m2`.
    var m3 = matrix.diff(m1, m2)

## [// Display using a table.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingatable.)

// Display using a table.
    var t = table.new(position.top_right, 1, 2, color.green)
    table.cell(t, 0, 0, "Difference between two matrices:")
    table.cell(t, 0, 1, str.tostring(m3))
Difference between a matrix and a scalar value
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.diff()` Example 2")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix with values `4`.
    var m1 = matrix.new<float>(2, 3, 4)

## [// Create a new matrix containing the difference between the `m1` matrix and the "int" value `1`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createanewmatrixcontainingthedifferencebetweenthem1matrixandtheintvalue1.)

// Create a new matrix containing the difference between the `m1` matrix and the "int" value `1`.
    var m2 = matrix.diff(m1, 1)

## [// Display using a table.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingatable.)

// Display using a table.
    var t = table.new(position.top_right, 1, 2, color.green)
    table.cell(t, 0, 0, "Difference between a matrix and a scalar:")
    table.cell(t, 0, 1, str.tostring(m2))
Returns
A new matrix object containing the difference between id2 and id1.
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.columns()
matrix.rows()
matrix.eigenvalues()2 overloads

## [The function returns an array containing the eigenvalues of a square matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsanarraycontainingtheeigenvaluesofasquarematrix.)

The function returns an array containing the eigenvalues of a square matrix.
Syntax & Overloads
matrix.eigenvalues(id) → array<float>
matrix.eigenvalues(id) → array<int>
Arguments
id (matrix<int/float>) A matrix object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.eigenvalues()` Example")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix.
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 2)
    matrix.set(m1, 0, 1, 4)
    matrix.set(m1, 1, 0, 6)
    matrix.set(m1, 1, 1, 8)

## [// Get the eigenvalues of the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Gettheeigenvaluesofthematrix.)

// Get the eigenvalues of the matrix.
    tr = matrix.eigenvalues(m1)

## [// Display matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displaymatrixelements.)

// Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Array of Eigenvalues:")
    table.cell(t, 1, 1, str.tostring(tr))
Returns
An array containing the eigenvalues of the id matrix.
Remarks
The function is calculated using "The Implicit QL Algorithm".
See also
matrix.new<type>()
matrix.set()
matrix.eigenvectors()
matrix.eigenvectors()2 overloads

## [Returns a matrix of eigenvectors, in which each column is an eigenvector of the id matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsamatrixofeigenvectorsinwhicheachcolumnisaneigenvectoroftheidmatrix.)

Returns a matrix of eigenvectors, in which each column is an eigenvector of the id matrix.
Syntax & Overloads
matrix.eigenvectors(id) → matrix<float>
matrix.eigenvectors(id) → matrix<int>
Arguments
id (matrix<int/float>) A matrix object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.eigenvectors()` Example")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix
    var m1 = matrix.new<int>(2, 2, 1)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 2)
    matrix.set(m1, 0, 1, 4)
    matrix.set(m1, 1, 0, 6)
    matrix.set(m1, 1, 1, 8)

## [// Get the eigenvectors of the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Gettheeigenvectorsofthematrix.)

// Get the eigenvectors of the matrix.
    m2 = matrix.eigenvectors(m1)

## [// Display matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displaymatrixelements.)

// Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix Elements:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Matrix Eigenvectors:")
    table.cell(t, 1, 1, str.tostring(m2))
Returns
A new matrix containing the eigenvectors of the id matrix.
Remarks
The function is calculated using "The Implicit QL Algorithm".
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.eigenvalues()
matrix.elements_count()

## [The function returns the total number of all matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthetotalnumberofallmatrixelements.)

The function returns the total number of all matrix elements.
Syntax
matrix.elements_count(id) → series int
Arguments
id (any matrix type) A matrix object.
See also
matrix.new<type>()
matrix.columns()
matrix.rows()
matrix.fill()

## [The function fills a rectangular area of the id matrix defined by the indices from_column to to_column (not including it) and from_row to to_row(not including it) with the value.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionfillsarectangularareaoftheidmatrixdefinedbytheindicesfrom_columntoto_columnnotincludingitandfrom_rowtoto_rownotincludingitwiththevalue.)

The function fills a rectangular area of the id matrix defined by the indices from_column to to_column (not including it) and from_row to to_row(not including it) with the value.
Syntax
matrix.fill(id, value, from_row, to_row, from_column, to_column) → void
Arguments
id (any matrix type) A matrix object.
value (series <type of the matrix's elements>) The value to fill with.
from_row (series int) Row index from which the fill will begin (inclusive). Optional. The default value is 0.
to_row (series int) Row index where the fill will end (not inclusive). Optional. The default value is matrix.rows().
from_column (series int) Column index from which the fill will begin (inclusive). Optional. The default value is 0.
to_column (series int) Column index where the fill will end (non inclusive). Optional. The default value is matrix.columns().
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.fill()` Example")

## [// Create a 4x5 "int" matrix containing values `0`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa4x5intmatrixcontainingvalues0.)

// Create a 4x5 "int" matrix containing values `0`.
m = matrix.new<float>(4, 5, 0)

## [// Fill the intersection of rows 1 to 2 and columns 2 to 3 of the matrix with `hl2` values.](https://www.tradingview.com/pine-script-reference/v6/#fun_Filltheintersectionofrows1to2andcolumns2to3ofthematrixwithhl2values.)

// Fill the intersection of rows 1 to 2 and columns 2 to 3 of the matrix with `hl2` values.
matrix.fill(m, hl2, 0, 2, 1, 3)

## [// Display using a label.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingalabel.)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m))
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.columns()
matrix.rows()
matrix.get()

## [The function returns the element with the specified index of the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnstheelementwiththespecifiedindexofthematrix.)

The function returns the element with the specified index of the matrix.
Syntax
matrix.get(id, row, column) → <matrix_type>
Arguments
id (any matrix type) A matrix object.
row (series int) Index of the required row.
column (series int) Index of the required column.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.get()` Example", "", true)

## [// Create a 2x3 "float" matrix from the `hl2` values.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x3floatmatrixfromthehl2values.)

// Create a 2x3 "float" matrix from the `hl2` values.
m = matrix.new<float>(2, 3, hl2)

## [// Return the value of the element at index [0, 0] of matrix `m`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnthevalueoftheelementatindex00ofmatrixm.)

// Return the value of the element at index [0, 0] of matrix `m`.
x = matrix.get(m, 0, 0)

## [plot(x)](https://www.tradingview.com/pine-script-reference/v6/#fun_plotx)

plot(x)
Returns
The value of the element at the row and column index of the id matrix.
Remarks
Indexing of the rows and columns starts at zero.
See also
matrix.new<type>()
matrix.set()
matrix.columns()
matrix.rows()
matrix.inv()2 overloads

## [The function returns the inverse of a square matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnstheinverseofasquarematrix.)

The function returns the inverse of a square matrix.
Syntax & Overloads
matrix.inv(id) → matrix<float>
matrix.inv(id) → matrix<int>
Arguments
id (matrix<int/float>) A matrix object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.inv()` Example")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix.
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)

## [// Inverse of the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Inverseofthematrix.)

// Inverse of the matrix.
    var m2 = matrix.inv(m1)

## [// Display matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displaymatrixelements.)

// Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Inverse matrix:")
    table.cell(t, 1, 1, str.tostring(m2))
Returns
A new matrix, which is the inverse of the id matrix.
Remarks
The function is calculated using the LU decomposition algorithm.
See also
matrix.new<type>()
matrix.set()
matrix.pinv()
matrix.copy()
str.tostring()
matrix.is_antidiagonal()

## [The function determines if the matrix is anti-diagonal (all elements outside the secondary diagonal are zero).](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctiondeterminesifthematrixisanti-diagonalallelementsoutsidethesecondarydiagonalarezero.)

The function determines if the matrix is anti-diagonal (all elements outside the secondary diagonal are zero).
Syntax
matrix.is_antidiagonal(id) → series bool
Arguments
id (matrix<int/float>) Matrix object to test.
Returns
Returns true if the id matrix is ​​anti-diagonal, false otherwise.
Remarks
Returns false with non-square matrices.
See also
matrix.new<type>()
matrix.set()
matrix.is_square()
matrix.is_identity()
matrix.is_diagonal()
matrix.is_antisymmetric()

## [The function determines if a matrix is antisymmetric (its transpose equals its negative).](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctiondeterminesifamatrixisantisymmetricitstransposeequalsitsnegative.)

The function determines if a matrix is antisymmetric (its transpose equals its negative).
Syntax
matrix.is_antisymmetric(id) → series bool
Arguments
id (matrix<int/float>) Matrix object to test.
Returns
Returns true, if the id matrix is antisymmetric, false otherwise.
Remarks
Returns false with non-square matrices.
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.is_square()
matrix.is_binary()

## [The function determines if the matrix is binary (when all elements of the matrix are 0 or 1).](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctiondeterminesifthematrixisbinarywhenallelementsofthematrixare0or1.)

The function determines if the matrix is binary (when all elements of the matrix are 0 or 1).
Syntax
matrix.is_binary(id) → series bool
Arguments
id (matrix<int/float>) Matrix object to test.
Returns
Returns true if the id matrix is binary, false otherwise.
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.is_diagonal()

## [The function determines if the matrix is diagonal (all elements outside the main diagonal are zero).](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctiondeterminesifthematrixisdiagonalallelementsoutsidethemaindiagonalarezero.)

The function determines if the matrix is diagonal (all elements outside the main diagonal are zero).
Syntax
matrix.is_diagonal(id) → series bool
Arguments
id (matrix<int/float>) Matrix object to test.
Returns
Returns true if the id matrix is diagonal, false otherwise.
Remarks
Returns false with non-square matrices.
See also
matrix.new<type>()
matrix.set()
matrix.is_square()
matrix.is_identity()
matrix.is_antidiagonal()
matrix.is_identity()

## [The function determines if a matrix is an identity matrix (elements with ones on the main diagonal and zeros elsewhere).](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctiondeterminesifamatrixisanidentitymatrixelementswithonesonthemaindiagonalandzeroselsewhere.)

The function determines if a matrix is an identity matrix (elements with ones on the main diagonal and zeros elsewhere).
Syntax
matrix.is_identity(id) → series bool
Arguments
id (matrix<int/float>) Matrix object to test.
Returns
Returns true if id is an identity matrix, false otherwise.
Remarks
Returns false with non-square matrices.
See also
matrix.new<type>()
matrix.is_square()
matrix.is_diagonal()
matrix.is_square()

## [The function determines if the matrix is square (it has the same number of rows and columns).](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctiondeterminesifthematrixissquareithasthesamenumberofrowsandcolumns.)

The function determines if the matrix is square (it has the same number of rows and columns).
Syntax
matrix.is_square(id) → series bool
Arguments
id (any matrix type) Matrix object to test.
Returns
Returns true if the id matrix is square, false otherwise.
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.columns()
matrix.rows()
matrix.is_stochastic()

## [The function determines if the matrix is stochastic.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctiondeterminesifthematrixisstochastic.)

The function determines if the matrix is stochastic.
Syntax
matrix.is_stochastic(id) → series bool
Arguments
id (matrix<int/float>) Matrix object to test.
Returns
Returns true if the id matrix is stochastic, false otherwise.
See also
matrix.new<type>()
matrix.set()
matrix.is_symmetric()

## [The function determines if a square matrix is symmetric (elements are symmetric with respect to the main diagonal).](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctiondeterminesifasquarematrixissymmetricelementsaresymmetricwithrespecttothemaindiagonal.)

The function determines if a square matrix is symmetric (elements are symmetric with respect to the main diagonal).
Syntax
matrix.is_symmetric(id) → series bool
Arguments
id (matrix<int/float>) Matrix object to test.
Returns
Returns true if the id matrix is symmetric, false otherwise.
Remarks
Returns false with non-square matrices.
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.is_square()
matrix.is_triangular()

## [The function determines if the matrix is triangular (if all elements above or below the main diagonal are zero).](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctiondeterminesifthematrixistriangularifallelementsaboveorbelowthemaindiagonalarezero.)

The function determines if the matrix is triangular (if all elements above or below the main diagonal are zero).
Syntax
matrix.is_triangular(id) → series bool
Arguments
id (matrix<int/float>) Matrix object to test.
Returns
Returns true if the id matrix is triangular, false otherwise.
Remarks
Returns false with non-square matrices.
See also
matrix.new<type>()
matrix.set()
matrix.is_square()
matrix.is_zero()

## [The function determines if all elements of the matrix are zero.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctiondeterminesifallelementsofthematrixarezero.)

The function determines if all elements of the matrix are zero.
Syntax
matrix.is_zero(id) → series bool
Arguments
id (matrix<int/float>) Matrix object to check.
Returns
Returns true if all elements of the id matrix are zero, false otherwise.
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.kron()2 overloads

## [The function returns the Kronecker product for the id1 and id2 matrices.](https://www.tradingview.com/pine-script-reference/v6/#fun_ThefunctionreturnstheKroneckerproductfortheid1andid2matrices.)

The function returns the Kronecker product for the id1 and id2 matrices.
Syntax & Overloads
matrix.kron(id1, id2) → matrix<float>
matrix.kron(id1, id2) → matrix<int>
Arguments
id1 (matrix<int/float>) First matrix object.
id2 (matrix<int/float>) Second matrix object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.kron()` Example")

## [// Display using a table.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingatable.)

// Display using a table.
if barstate.islastconfirmedhistory
    // Create two matrices with default values `1` and `2`.
    var m1 = matrix.new<float>(2, 2, 1)
    var m2 = matrix.new<float>(2, 2, 2)

## [// Calculate the Kronecker product of the matrices.](https://www.tradingview.com/pine-script-reference/v6/#fun_CalculatetheKroneckerproductofthematrices.)

// Calculate the Kronecker product of the matrices.
    var m3 = matrix.kron(m1, m2)

## [// Display matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displaymatrixelements.)

// Display matrix elements.
    var t = table.new(position.top_right, 5, 2, color.green)
    table.cell(t, 0, 0, "Matrix 1:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 1, "⊗")
    table.cell(t, 2, 0, "Matrix 2:")
    table.cell(t, 2, 1, str.tostring(m2))
    table.cell(t, 3, 1, "=")
    table.cell(t, 4, 0, "Kronecker product:")
    table.cell(t, 4, 1, str.tostring(m3))
Returns
A new matrix containing the Kronecker product of id1 and id2.
See also
matrix.new<type>()
matrix.mult()
str.tostring()
table.new()
matrix.max()2 overloads

## [The function returns the largest value from the matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthelargestvaluefromthematrixelements.)

The function returns the largest value from the matrix elements.
Syntax & Overloads
matrix.max(id) → series float
matrix.max(id) → series int
Arguments
id (matrix<int/float>) A matrix object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.max()` Example")

## [// Create a 2x2 matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x2matrix.)

// Create a 2x2 matrix.
var m = matrix.new<int>(2, 2, na)
// Fill the matrix with values.
matrix.set(m, 0, 0, 1)
matrix.set(m, 0, 1, 2)
matrix.set(m, 1, 0, 3)
matrix.set(m, 1, 1, 4)

## [// Get the maximum value in the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Getthemaximumvalueinthematrix.)

// Get the maximum value in the matrix.
var x = matrix.max(m)

## [plot(x, 'Matrix maximum value')](https://www.tradingview.com/pine-script-reference/v6/#fun_plotxMatrixmaximumvalue)

plot(x, 'Matrix maximum value')
Returns
The maximum value from the id matrix.
See also
matrix.new<type>()
matrix.min()
matrix.avg()
matrix.sort()
matrix.median()2 overloads

## [The function calculates the median ("the middle" value) of matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncalculatesthemedianthemiddlevalueofmatrixelements.)

The function calculates the median ("the middle" value) of matrix elements.
Syntax & Overloads
matrix.median(id) → series float
matrix.median(id) → series int
Arguments
id (matrix<int/float>) A matrix object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.median()` Example")

## [// Create a 2x2 matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x2matrix.)

// Create a 2x2 matrix.
m = matrix.new<int>(2, 2, na)
// Fill the matrix with values.
matrix.set(m, 0, 0, 1)
matrix.set(m, 0, 1, 2)
matrix.set(m, 1, 0, 3)
matrix.set(m, 1, 1, 4)

## [// Get the median of the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Getthemedianofthematrix.)

// Get the median of the matrix.
x = matrix.median(m)

## [plot(x, 'Median of the matrix')](https://www.tradingview.com/pine-script-reference/v6/#fun_plotxMedianofthematrix)

plot(x, 'Median of the matrix')
Remarks
Note that na elements of the matrix are not considered when calculating the median.
See also
matrix.new<type>()
matrix.mode()
matrix.sort()
matrix.avg()
matrix.min()2 overloads

## [The function returns the smallest value from the matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthesmallestvaluefromthematrixelements.)

The function returns the smallest value from the matrix elements.
Syntax & Overloads
matrix.min(id) → series float
matrix.min(id) → series int
Arguments
id (matrix<int/float>) A matrix object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.min()` Example")

## [// Create a 2x2 matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x2matrix.)

// Create a 2x2 matrix.
var m = matrix.new<int>(2, 2, na)
// Fill the matrix with values.
matrix.set(m, 0, 0, 1)
matrix.set(m, 0, 1, 2)
matrix.set(m, 1, 0, 3)
matrix.set(m, 1, 1, 4)

## [// Get the minimum value from the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Gettheminimumvaluefromthematrix.)

// Get the minimum value from the matrix.
var x = matrix.min(m)

## [plot(x, 'Matrix minimum value')](https://www.tradingview.com/pine-script-reference/v6/#fun_plotxMatrixminimumvalue)

plot(x, 'Matrix minimum value')
Returns
The smallest value from the id matrix.
See also
matrix.new<type>()
matrix.max()
matrix.avg()
matrix.sort()
matrix.mode()2 overloads

## [The function calculates the mode of the matrix, which is the most frequently occurring value from the matrix elements. When there are multiple values occurring equally frequently, the function returns the smallest of those values.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncalculatesthemodeofthematrixwhichisthemostfrequentlyoccurringvaluefromthematrixelements.Whentherearemultiplevaluesoccurringequallyfrequentlythefunctionreturnsthesmallestofthosevalues.)

The function calculates the mode of the matrix, which is the most frequently occurring value from the matrix elements. When there are multiple values occurring equally frequently, the function returns the smallest of those values.
Syntax & Overloads
matrix.mode(id) → series float
matrix.mode(id) → series int
Arguments
id (matrix<int/float>) A matrix object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.mode()` Example")

## [// Create a 2x2 matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x2matrix.)

// Create a 2x2 matrix.
var m = matrix.new<int>(2, 2, na)
// Fill the matrix with values.
matrix.set(m, 0, 0, 0)
matrix.set(m, 0, 1, 0)
matrix.set(m, 1, 0, 1)
matrix.set(m, 1, 1, 1)

## [// Get the mode of the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Getthemodeofthematrix.)

// Get the mode of the matrix.
var x = matrix.mode(m)

## [plot(x, 'Mode of the matrix')](https://www.tradingview.com/pine-script-reference/v6/#fun_plotxModeofthematrix)

plot(x, 'Mode of the matrix')
Returns
The most frequently occurring value from the id matrix. If none exists, returns the smallest value instead.
Remarks
Note that na elements of the matrix are not considered when calculating the mode.
See also
matrix.new<type>()
matrix.set()
matrix.median()
matrix.sort()
matrix.avg()
matrix.mult()4 overloads

## [The function returns a new matrix resulting from the product between the matrices id1 and id2, or between an id1 matrix and an id2 scalar (a numerical value), or between an id1 matrix and an id2 vector (an array of values).](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsanewmatrixresultingfromtheproductbetweenthematricesid1andid2orbetweenanid1matrixandanid2scalaranumericalvalueorbetweenanid1matrixandanid2vectoranarrayofvalues.)

The function returns a new matrix resulting from the product between the matrices id1 and id2, or between an id1 matrix and an id2 scalar (a numerical value), or between an id1 matrix and an id2 vector (an array of values).
Syntax & Overloads
matrix.mult(id1, id2) → array<int>
matrix.mult(id1, id2) → array<float>
matrix.mult(id1, id2) → matrix<int>
matrix.mult(id1, id2) → matrix<float>
Arguments
id1 (matrix<int>) First matrix object.
id2 (array<int>) Second matrix object, value or array.
Product of two matrices
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.mult()` Example 1")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 6x2 matrix containing values `5`.
    var m1 = matrix.new<float>(6, 2, 5)
    // Create a 2x3 matrix containing values `4`.
    // Note that it must have the same quantity of rows as there are columns in the first matrix.
    var m2 = matrix.new<float>(2, 3, 4)
    // Create a new matrix from the multiplication of the two matrices.
    var m3 = matrix.mult(m1, m2)

## [// Display using a table.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingatable.)

// Display using a table.
    var t = table.new(position.top_right, 1, 2, color.green)
    table.cell(t, 0, 0, "Product of two matrices:")
    table.cell(t, 0, 1, str.tostring(m3))
Product of a matrix and a scalar
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.mult()` Example 2")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix containing values `4`.
    var m1 = matrix.new<float>(2, 3, 4)

## [// Create a new matrix from the product of the two matrices.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createanewmatrixfromtheproductofthetwomatrices.)

// Create a new matrix from the product of the two matrices.
    scalar = 5
    var m2 = matrix.mult(m1, scalar)

## [// Display using a table.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingatable.)

// Display using a table.
    var t = table.new(position.top_right, 5, 2, color.green)
    table.cell(t, 0, 0, "Matrix 1:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 1, "x")
    table.cell(t, 2, 0, "Scalar:")
    table.cell(t, 2, 1, str.tostring(scalar))
    table.cell(t, 3, 1, "=")
    table.cell(t, 4, 0, "Matrix 2:")
    table.cell(t, 4, 1, str.tostring(m2))
Product of a matrix and an array vector
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.mult()` Example 3")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix containing values `4`.
    var m1 = matrix.new<int>(2, 3, 4)

## [// Create an array of three elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createanarrayofthreeelements.)

// Create an array of three elements.
    var array<int> a = array.from(1, 1, 1)

## [// Create a new matrix containing the product of the `m1` matrix and the `a` array.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createanewmatrixcontainingtheproductofthem1matrixandtheaarray.)

// Create a new matrix containing the product of the `m1` matrix and the `a` array.
    var m3 = matrix.mult(m1, a)

## [// Display using a table.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingatable.)

// Display using a table.
    var t = table.new(position.top_right, 5, 2, color.green)
    table.cell(t, 0, 0, "Matrix 1:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 1, "x")
    table.cell(t, 2, 0, "Value:")
    table.cell(t, 2, 1, str.tostring(a, " "))
    table.cell(t, 3, 1, "=")
    table.cell(t, 4, 0, "Matrix 3:")
    table.cell(t, 4, 1, str.tostring(m3))
Returns
A new matrix object containing the product of id2 and id1.
See also
matrix.new<type>()
matrix.sum()
matrix.diff()
matrix.new<type>()

## [The function creates a new matrix object. A matrix is a two-dimensional data structure containing rows and columns. All elements in the matrix must be of the type specified in the type template ("<type>").](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncreatesanewmatrixobject.Amatrixisatwo-dimensionaldatastructurecontainingrowsandcolumns.Allelementsinthematrixmustbeofthetypespecifiedinthetypetemplatetype.)

The function creates a new matrix object. A matrix is a two-dimensional data structure containing rows and columns. All elements in the matrix must be of the type specified in the type template ("<type>").
Syntax
matrix.new<type>(rows, columns, initial_value) → matrix<type>
Arguments
rows (series int) Initial row count of the matrix. Optional. The default value is 0.
columns (series int) Initial column count of the matrix. Optional. The default value is 0.
initial_value (<matrix_type>) Initial value of all matrix elements. Optional. The default is 'na'.
Create a matrix of elements with the same initial value
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.new<type>()` Example 1")

## [// Create a 2x3 (2 rows x 3 columns) "int" matrix with values zero.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x32rowsx3columnsintmatrixwithvalueszero.)

// Create a 2x3 (2 rows x 3 columns) "int" matrix with values zero.
var m = matrix.new<int>(2, 3, 0)

## [// Display using a label.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingalabel.)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m))
Create a matrix from array values
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.new<type>()` Example 2")

## [// Function to create a matrix whose rows are filled with array values.](https://www.tradingview.com/pine-script-reference/v6/#fun_Functiontocreateamatrixwhoserowsarefilledwitharrayvalues.)

// Function to create a matrix whose rows are filled with array values.
matrixFromArray(int rows, int columns, array<float> data) =>
    m = matrix.new<float>(rows, columns)
    for i = 0 to rows <= 0 ? na : rows - 1
        for j = 0 to columns <= 0 ? na : columns - 1
            matrix.set(m, i, j, array.get(data, i * columns + j))
    m

## [// Create a 3x3 matrix from an array of values.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa3x3matrixfromanarrayofvalues.)

// Create a 3x3 matrix from an array of values.
var m1 = matrixFromArray(3, 3, array.from(1, 2, 3, 4, 5, 6, 7, 8, 9))
// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m1))
Create a matrix from an input.text_area() field
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.new<type>()` Example 3")

## [// Function to create a matrix from a text string.](https://www.tradingview.com/pine-script-reference/v6/#fun_Functiontocreateamatrixfromatextstring.)

// Function to create a matrix from a text string.
// Values in a row must be separated by a space. Each line is one row.
matrixFromInputArea(stringOfValues) =>
    var rowsArray = str.split(stringOfValues, "\n")
    var rows = array.size(rowsArray)
    var cols = array.size(str.split(array.get(rowsArray, 0), " "))
    var matrix = matrix.new<float>(rows, cols, na)
    row = 0
    for rowString in rowsArray
        col = 0
        values = str.split(rowString, " ")
        for val in values
            matrix.set(matrix, row, col, str.tonumber(val))
            col += 1
        row += 1
    matrix

## [stringInput = input.text_area("1 2 3\n4 5 6\n7 8 9")](https://www.tradingview.com/pine-script-reference/v6/#fun_stringInputinput.text_area123n456n789)

stringInput = input.text_area("1 2 3\n4 5 6\n7 8 9")
var m = matrixFromInputArea(stringInput)

## [// Display using a label.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingalabel.)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m))
Create matrix from random values
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.new<type>()` Example 4")

## [// Function to create a matrix with random values (0.0 to 1.0).](https://www.tradingview.com/pine-script-reference/v6/#fun_Functiontocreateamatrixwithrandomvalues0.0to1.0.)

// Function to create a matrix with random values (0.0 to 1.0).
matrixRandom(int rows, int columns)=>
    result = matrix.new<float>(rows, columns)
    for i = 0 to rows - 1
        for j = 0 to columns - 1
            matrix.set(result, i, j, math.random())
    result

## [// Create a 2x3 matrix with random values.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x3matrixwithrandomvalues.)

// Create a 2x3 matrix with random values.
var m = matrixRandom(2, 3)

## [// Display using a label.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingalabel.)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m))
Returns
The ID of the new matrix object.
See also
matrix.set()
matrix.fill()
matrix.columns()
matrix.rows()
array.new<type>()
matrix.pinv()2 overloads

## [The function returns the pseudoinverse of a matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthepseudoinverseofamatrix.)

The function returns the pseudoinverse of a matrix.
Syntax & Overloads
matrix.pinv(id) → matrix<float>
matrix.pinv(id) → matrix<int>
Arguments
id (matrix<int/float>) A matrix object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.pinv()` Example")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix.
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)

## [// Pseudoinverse of the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Pseudoinverseofthematrix.)

// Pseudoinverse of the matrix.
    var m2 = matrix.pinv(m1)

## [// Display matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displaymatrixelements.)

// Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Pseudoinverse matrix:")
    table.cell(t, 1, 1, str.tostring(m2))
Returns
A new matrix containing the pseudoinverse of the id matrix.
Remarks
The function is calculated using a Moore–Penrose inverse formula based on singular-value decomposition of a matrix. For non-singular square matrices this function returns the result of matrix.inv().
See also
matrix.new<type>()
matrix.set()
matrix.inv()
matrix.pow()2 overloads

## [The function calculates the product of the matrix by itself power times.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncalculatestheproductofthematrixbyitselfpowertimes.)

The function calculates the product of the matrix by itself power times.
Syntax & Overloads
matrix.pow(id, power) → matrix<float>
matrix.pow(id, power) → matrix<int>
Arguments
id (matrix<int/float>) A matrix object.
power (series int) The number of times the matrix will be multiplied by itself.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.pow()` Example")

## [// Display using a table.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingatable.)

// Display using a table.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix.
    var m1 = matrix.new<int>(2, 2, 2)
    // Calculate the power of three of the matrix.
    var m2 = matrix.pow(m1, 3)

## [// Display matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displaymatrixelements.)

// Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Matrix³:")
    table.cell(t, 1, 1, str.tostring(m2))
Returns
The product of the id matrix by itself power times.
See also
matrix.new<type>()
matrix.set()
matrix.mult()
matrix.rank()

## [The function calculates the rank of the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncalculatestherankofthematrix.)

The function calculates the rank of the matrix.
Syntax
matrix.rank(id) → series int
Arguments
id (any matrix type) A matrix object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.rank()` Example")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix.
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)

## [// Get the rank of the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Gettherankofthematrix.)

// Get the rank of the matrix.
    r = matrix.rank(m1)

## [// Display matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displaymatrixelements.)

// Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Rank of the matrix:")
    table.cell(t, 1, 1, str.tostring(r))
Returns
The rank of the id matrix.
See also
matrix.new<type>()
matrix.set()
str.tostring()
matrix.remove_col()

## [The function removes the column at column index of the id matrix and returns an array containing the removed column's values.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionremovesthecolumnatcolumnindexoftheidmatrixandreturnsanarraycontainingtheremovedcolumnsvalues.)

The function removes the column at column index of the id matrix and returns an array containing the removed column's values.
Syntax
matrix.remove_col(id, column) → array<type>
Arguments
id (any matrix type) A matrix object.
column (series int) The index of the column to be removed. Optional. The default value is matrix.columns().
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("matrix_remove_col", overlay = true)

## [// Create a 2x2 matrix with ones.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x2matrixwithones.)

// Create a 2x2 matrix with ones.
var matrixOrig = matrix.new<int>(2, 2, 1)

## [// Set values to the 'matrixOrig' matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_SetvaluestothematrixOrigmatrix.)

// Set values to the 'matrixOrig' matrix.
matrix.set(matrixOrig, 0, 1, 2)
matrix.set(matrixOrig, 1, 0, 3)
matrix.set(matrixOrig, 1, 1, 4)

## [// Create a copy of the 'matrixOrig' matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_CreateacopyofthematrixOrigmatrix.)

// Create a copy of the 'matrixOrig' matrix.
matrixCopy = matrix.copy(matrixOrig)

## [// Remove the first column from the `matrixCopy` matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_RemovethefirstcolumnfromthematrixCopymatrix.)

// Remove the first column from the `matrixCopy` matrix.
arr = matrix.remove_col(matrixCopy, 0)

## [// Display matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displaymatrixelements.)

// Display matrix elements.
if barstate.islastconfirmedhistory
    var t = table.new(position.top_right, 3, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(matrixOrig))
    table.cell(t, 1, 0, "Removed Elements:")
    table.cell(t, 1, 1, str.tostring(arr))
    table.cell(t, 2, 0, "Result Matrix:")
    table.cell(t, 2, 1, str.tostring(matrixCopy))
Returns
An array containing the elements of the column removed from the id matrix.
Remarks
Indexing of rows and columns starts at zero. It is far more efficient to declare matrices with explicit dimensions than to build them by adding or removing columns. Deleting a column is also much slower than deleting a row with the matrix.remove_row() function.
See also
matrix.new<type>()
matrix.set()
matrix.copy()
matrix.remove_row()
matrix.remove_row()

## [The function removes the row at row index of the id matrix and returns an array containing the removed row's values.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionremovestherowatrowindexoftheidmatrixandreturnsanarraycontainingtheremovedrowsvalues.)

The function removes the row at row index of the id matrix and returns an array containing the removed row's values.
Syntax
matrix.remove_row(id, row) → array<type>
Arguments
id (any matrix type) A matrix object.
row (series int) The index of the row to be deleted. Optional. The default value is matrix.rows().
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("matrix_remove_row", overlay = true)

## [// Create a 2x2 "int" matrix containing values `1`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x2intmatrixcontainingvalues1.)

// Create a 2x2 "int" matrix containing values `1`.
var matrixOrig = matrix.new<int>(2, 2, 1)

## [// Set values to the 'matrixOrig' matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_SetvaluestothematrixOrigmatrix.)

// Set values to the 'matrixOrig' matrix.
matrix.set(matrixOrig, 0, 1, 2)
matrix.set(matrixOrig, 1, 0, 3)
matrix.set(matrixOrig, 1, 1, 4)

## [// Create a copy of the 'matrixOrig' matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_CreateacopyofthematrixOrigmatrix.)

// Create a copy of the 'matrixOrig' matrix.
matrixCopy = matrix.copy(matrixOrig)

## [// Remove the first row from the matrix `matrixCopy`.](https://www.tradingview.com/pine-script-reference/v6/#fun_RemovethefirstrowfromthematrixmatrixCopy.)

// Remove the first row from the matrix `matrixCopy`.
arr = matrix.remove_row(matrixCopy, 0)

## [// Display matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displaymatrixelements.)

// Display matrix elements.
if barstate.islastconfirmedhistory
    var t = table.new(position.top_right, 3, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(matrixOrig))
    table.cell(t, 1, 0, "Removed Elements:")
    table.cell(t, 1, 1, str.tostring(arr))
    table.cell(t, 2, 0, "Result Matrix:")
    table.cell(t, 2, 1, str.tostring(matrixCopy))
Returns
An array containing the elements of the row removed from the id matrix.
Remarks
Indexing of rows and columns starts at zero. It is far more efficient to declare matrices with explicit dimensions than to build them by adding or removing rows.
See also
matrix.new<type>()
matrix.set()
matrix.copy()
matrix.remove_col()
matrix.reshape()

## [The function rebuilds the id matrix to rows x cols dimensions.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionrebuildstheidmatrixtorowsxcolsdimensions.)

The function rebuilds the id matrix to rows x cols dimensions.
Syntax
matrix.reshape(id, rows, columns) → void
Arguments
id (any matrix type) A matrix object.
rows (series int) The number of rows of the reshaped matrix.
columns (series int) The number of columns of the reshaped matrix.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.reshape()` Example")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix.
    var m1 = matrix.new<float>(2, 3)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 0, 2, 3)
    matrix.set(m1, 1, 0, 4)
    matrix.set(m1, 1, 1, 5)
    matrix.set(m1, 1, 2, 6)

## [// Copy the matrix to a new one.](https://www.tradingview.com/pine-script-reference/v6/#fun_Copythematrixtoanewone.)

// Copy the matrix to a new one.
    var m2 = matrix.copy(m1)

## [// Reshape the copy to a 3x2.](https://www.tradingview.com/pine-script-reference/v6/#fun_Reshapethecopytoa3x2.)

// Reshape the copy to a 3x2.
    matrix.reshape(m2, 3, 2)

## [// Display using a table.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingatable.)

// Display using a table.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Reshaped matrix:")
    table.cell(t, 1, 1, str.tostring(m2))
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.add_row()
matrix.add_col()
matrix.reverse()

## [The function reverses the order of rows and columns in the matrix id. The first row and first column become the last, and the last become the first.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreversestheorderofrowsandcolumnsinthematrixid.Thefirstrowandfirstcolumnbecomethelastandthelastbecomethefirst.)

The function reverses the order of rows and columns in the matrix id. The first row and first column become the last, and the last become the first.
Syntax
matrix.reverse(id) → void
Arguments
id (any matrix type) A matrix object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.reverse()` Example")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Copy the matrix to a new one.
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)

## [// Copy matrix elements to a new matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Copymatrixelementstoanewmatrix.)

// Copy matrix elements to a new matrix.
    var m2 = matrix.copy(m1)

## [// Reverse the `m2` copy of the original matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Reversethem2copyoftheoriginalmatrix.)

// Reverse the `m2` copy of the original matrix.
    matrix.reverse(m2)

## [// Display using a table.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingatable.)

// Display using a table.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Reversed matrix:")
    table.cell(t, 1, 1, str.tostring(m2))
See also
matrix.new<type>()
matrix.set()
matrix.columns()
matrix.rows()
matrix.reshape()
matrix.row()

## [The function creates a one-dimensional array from the elements of a matrix row.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncreatesaone-dimensionalarrayfromtheelementsofamatrixrow.)

The function creates a one-dimensional array from the elements of a matrix row.
Syntax
matrix.row(id, row) → array<type>
Arguments
id (any matrix type) A matrix object.
row (series int) Index of the required row.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.row()` Example", "", true)

## [// Create a 2x3 "float" matrix from `hlc3` values.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x3floatmatrixfromhlc3values.)

// Create a 2x3 "float" matrix from `hlc3` values.
m = matrix.new<float>(2, 3, hlc3)

## [// Return an array with the values of the first row of the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnanarraywiththevaluesofthefirstrowofthematrix.)

// Return an array with the values of the first row of the matrix.
a = matrix.row(m, 0)

## [// Plot the first value from the array `a`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Plotthefirstvaluefromthearraya.)

// Plot the first value from the array `a`.
plot(array.get(a, 0))
Returns
An array ID containing the row values of the id matrix.
Remarks
Indexing of rows starts at 0.
See also
matrix.new<type>()
matrix.get()
array.get()
matrix.col()
matrix.rows()
matrix.rows()

## [The function returns the number of rows in the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsthenumberofrowsinthematrix.)

The function returns the number of rows in the matrix.
Syntax
matrix.rows(id) → series int
Arguments
id (any matrix type) A matrix object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.rows()` Example")

## [// Create a 2x6 matrix with values `0`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x6matrixwithvalues0.)

// Create a 2x6 matrix with values `0`.
var m = matrix.new<int>(2, 6, 0)

## [// Get the quantity of rows in the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Getthequantityofrowsinthematrix.)

// Get the quantity of rows in the matrix.
var x = matrix.rows(m)

## [// Display using a label.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingalabel.)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, "Rows: " + str.tostring(x) + "\n" + str.tostring(m))
Returns
The number of rows in the matrix id.
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.columns()
matrix.row()
matrix.set()

## [The function assigns value to the element at the row and column of the id matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionassignsvaluetotheelementattherowandcolumnoftheidmatrix.)

The function assigns value to the element at the row and column of the id matrix.
Syntax
matrix.set(id, row, column, value) → void
Arguments
id (any matrix type) A matrix object.
row (series int) The row index of the element to be modified.
column (series int) The column index of the element to be modified.
value (series <type of the matrix's elements>) The new value to be set.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.set()` Example")

## [// Create a 2x3 "int" matrix containing values `4`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x3intmatrixcontainingvalues4.)

// Create a 2x3 "int" matrix containing values `4`.
m = matrix.new<int>(2, 3, 4)

## [// Replace the value of element at row 1 and column 2 with value `3`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Replacethevalueofelementatrow1andcolumn2withvalue3.)

// Replace the value of element at row 1 and column 2 with value `3`.
matrix.set(m, 0, 1, 3)

## [// Display using a label.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingalabel.)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m))
See also
matrix.new<type>()
matrix.get()
matrix.columns()
matrix.rows()
matrix.sort()2 overloads

## [The function rearranges the rows in the id matrix following the sorted order of the values in the column.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionrearrangestherowsintheidmatrixfollowingthesortedorderofthevaluesinthecolumn.)

The function rearranges the rows in the id matrix following the sorted order of the values in the column.
Syntax & Overloads
matrix.sort(id, column, order) → void
matrix.sort(id, column, order, sort_field) → void
Arguments
id (matrix<int/float/string>) A matrix object to be sorted.
column (series int) Index of the column whose sorted values determine the new order of rows. Optional. The default value is 0.
order (series sort_order) The sort order. Possible values: order.ascending (default), order.descending.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.sort()` Example")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix.
    var m1 = matrix.new<float>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 3)
    matrix.set(m1, 0, 1, 4)
    matrix.set(m1, 1, 0, 1)
    matrix.set(m1, 1, 1, 2)

## [// Copy the matrix to a new one.](https://www.tradingview.com/pine-script-reference/v6/#fun_Copythematrixtoanewone.)

// Copy the matrix to a new one.
    var m2 = matrix.copy(m1)
    // Sort the rows of `m2` using the default arguments (first column and ascending order).
    matrix.sort(m2)

## [// Display using a table.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingatable.)

// Display using a table.
    if barstate.islastconfirmedhistory
        var t = table.new(position.top_right, 2, 2, color.green)
        table.cell(t, 0, 0, "Original matrix:")
        table.cell(t, 0, 1, str.tostring(m1))
        table.cell(t, 1, 0, "Sorted matrix:")
        table.cell(t, 1, 1, str.tostring(m2))
See also
matrix.new<type>()
matrix.max()
matrix.min()
matrix.avg()
matrix.submatrix()

## [The function extracts a submatrix of the id matrix within the specified indices.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionextractsasubmatrixoftheidmatrixwithinthespecifiedindices.)

The function extracts a submatrix of the id matrix within the specified indices.
Syntax
matrix.submatrix(id, from_row, to_row, from_column, to_column) → matrix<type>
Arguments
id (any matrix type) A matrix object.
from_row (series int) Index of the row from which the extraction will begin (inclusive). Optional. The default value is 0.
to_row (series int) Index of the row where the extraction will end (non inclusive). Optional. The default value is matrix.rows().
from_column (series int) Index of the column from which the extraction will begin (inclusive). Optional. The default value is 0.
to_column (series int) Index of the column where the extraction will end (non inclusive). Optional. The default value is matrix.columns().
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.submatrix()` Example")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix matrix with values `0`.
    var m1 = matrix.new<int>(2, 3, 0)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 0, 2, 3)
    matrix.set(m1, 1, 0, 4)
    matrix.set(m1, 1, 1, 5)
    matrix.set(m1, 1, 2, 6)

## [// Create a 2x2 submatrix of the `m1` matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createa2x2submatrixofthem1matrix.)

// Create a 2x2 submatrix of the `m1` matrix.
    var m2 = matrix.submatrix(m1, 0, 2, 1, 3)

## [// Display using a table.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingatable.)

// Display using a table.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Submatrix:")
    table.cell(t, 1, 1, str.tostring(m2))
Returns
A new matrix object containing the submatrix of the id matrix defined by the from_row, to_row, from_column and to_column indices.
Remarks
Indexing of the rows and columns starts at zero.
See also
matrix.new<type>()
matrix.set()
matrix.row()
matrix.col()
matrix.reshape()
matrix.sum()2 overloads

## [The function returns a new matrix resulting from the sum of two matrices id1 and id2, or of an id1 matrix and an id2 scalar (a numerical value).](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionreturnsanewmatrixresultingfromthesumoftwomatricesid1andid2orofanid1matrixandanid2scalaranumericalvalue.)

The function returns a new matrix resulting from the sum of two matrices id1 and id2, or of an id1 matrix and an id2 scalar (a numerical value).
Syntax & Overloads
matrix.sum(id1, id2) → matrix<int>
matrix.sum(id1, id2) → matrix<float>
Arguments
id1 (matrix<int>) First matrix object.
id2 (series int/float/matrix<int>) Second matrix object, or scalar value.
Sum of two matrices
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.sum()` Example 1")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix containing values `5`.
    var m1 = matrix.new<float>(2, 3, 5)
    // Create a 2x3 matrix containing values `4`.
    var m2 = matrix.new<float>(2, 3, 4)
    // Create a new matrix that sums matrices `m1` and `m2`.
    var m3 = matrix.sum(m1, m2)

## [// Display using a table.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingatable.)

// Display using a table.
    var t = table.new(position.top_right, 1, 2, color.green)
    table.cell(t, 0, 0, "Sum of two matrices:")
    table.cell(t, 0, 1, str.tostring(m3))
Sum of a matrix and scalar
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.sum()` Example 2")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix with values `4`.
    var m1 = matrix.new<float>(2, 3, 4)

## [// Create a new matrix containing the sum of the `m1` matrix with the "int" value `1`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createanewmatrixcontainingthesumofthem1matrixwiththeintvalue1.)

// Create a new matrix containing the sum of the `m1` matrix with the "int" value `1`.
    var m2 = matrix.sum(m1, 1)

## [// Display using a table.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingatable.)

// Display using a table.
    var t = table.new(position.top_right, 1, 2, color.green)
    table.cell(t, 0, 0, "Sum of a matrix and a scalar:")
    table.cell(t, 0, 1, str.tostring(m2))
Returns
A new matrix object containing the sum of id2 and id1.
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.columns()
matrix.rows()
matrix.swap_columns()

## [The function swaps the columns at the index column1 and column2 in the id matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionswapsthecolumnsattheindexcolumn1andcolumn2intheidmatrix.)

The function swaps the columns at the index column1 and column2 in the id matrix.
Syntax
matrix.swap_columns(id, column1, column2) → void
Arguments
id (any matrix type) A matrix object.
column1 (series int) Index of the first column to be swapped.
column2 (series int) Index of the second column to be swapped.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.swap_columns()` Example")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix with ‘na’ values.
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)

## [// Copy the matrix to a new one.](https://www.tradingview.com/pine-script-reference/v6/#fun_Copythematrixtoanewone.)

// Copy the matrix to a new one.
    var m2 = matrix.copy(m1)

## [// Swap the first and second columns of the matrix copy.](https://www.tradingview.com/pine-script-reference/v6/#fun_Swapthefirstandsecondcolumnsofthematrixcopy.)

// Swap the first and second columns of the matrix copy.
    matrix.swap_columns(m2, 0, 1)

## [// Display using a table.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingatable.)

// Display using a table.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Swapped columns in copy:")
    table.cell(t, 1, 1, str.tostring(m2))
Remarks
Indexing of the rows and columns starts at zero.
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.columns()
matrix.rows()
matrix.swap_rows()

## [The function swaps the rows at the index row1 and row2 in the id matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctionswapstherowsattheindexrow1androw2intheidmatrix.)

The function swaps the rows at the index row1 and row2 in the id matrix.
Syntax
matrix.swap_rows(id, row1, row2) → void
Arguments
id (any matrix type) A matrix object.
row1 (series int) Index of the first row to be swapped.
row2 (series int) Index of the second row to be swapped.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.swap_rows()` Example")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 3x2 matrix with ‘na’ values.
    var m1 = matrix.new<int>(3, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)
    matrix.set(m1, 2, 0, 5)
    matrix.set(m1, 2, 1, 6)

## [// Copy the matrix to a new one.](https://www.tradingview.com/pine-script-reference/v6/#fun_Copythematrixtoanewone.)

// Copy the matrix to a new one.
    var m2 = matrix.copy(m1)

## [// Swap the first and second rows of the matrix copy.](https://www.tradingview.com/pine-script-reference/v6/#fun_Swapthefirstandsecondrowsofthematrixcopy.)

// Swap the first and second rows of the matrix copy.
    matrix.swap_rows(m2, 0, 1)

## [// Display using a table.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingatable.)

// Display using a table.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Swapped rows in copy:")
    table.cell(t, 1, 1, str.tostring(m2))
Remarks
Indexing of the rows and columns starts at zero.
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.columns()
matrix.swap_columns()
matrix.trace()2 overloads

## [The function calculates the trace of a matrix (the sum of the main diagonal's elements).](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncalculatesthetraceofamatrixthesumofthemaindiagonalselements.)

The function calculates the trace of a matrix (the sum of the main diagonal's elements).
Syntax & Overloads
matrix.trace(id) → series float
matrix.trace(id) → series int
Arguments
id (matrix<int/float>) A matrix object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.trace()` Example")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix.
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)

## [// Get the trace of the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Getthetraceofthematrix.)

// Get the trace of the matrix.
    tr = matrix.trace(m1)

## [// Display matrix elements.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displaymatrixelements.)

// Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Trace of the matrix:")
    table.cell(t, 1, 1, str.tostring(tr))
Returns
The trace of the id matrix.
See also
matrix.new<type>()
matrix.get()
matrix.set()
matrix.columns()
matrix.rows()
matrix.transpose()

## [The function creates a new, transposed version of the id. This interchanges the row and column index of each element.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thefunctioncreatesanewtransposedversionoftheid.Thisinterchangestherowandcolumnindexofeachelement.)

The function creates a new, transposed version of the id. This interchanges the row and column index of each element.
Syntax
matrix.transpose(id) → matrix<type>
Arguments
id (any matrix type) A matrix object.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`matrix.transpose()` Example")

## [// For efficiency, execute this code only once.](https://www.tradingview.com/pine-script-reference/v6/#fun_Forefficiencyexecutethiscodeonlyonce.)

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix.
    var m1 = matrix.new<float>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)

## [// Create a transpose of the matrix.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createatransposeofthematrix.)

// Create a transpose of the matrix.
    var m2 = matrix.transpose(m1)

## [// Display using a table.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayusingatable.)

// Display using a table.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Transposed matrix:")
    table.cell(t, 1, 1, str.tostring(m2))
Returns
A new matrix containing the transposed version of the id matrix.
See also
matrix.new<type>()
matrix.set()
matrix.columns()
matrix.rows()
matrix.reshape()
matrix.reverse()
max_bars_back()

## [Function sets the maximum number of bars that is available for historical reference of a given built-in or user variable. When operator '[]' is applied to a variable - it is a reference to a historical value of that variable.](https://www.tradingview.com/pine-script-reference/v6/#fun_Functionsetsthemaximumnumberofbarsthatisavailableforhistoricalreferenceofagivenbuilt-inoruservariable.Whenoperatorisappliedtoavariable-itisareferencetoahistoricalvalueofthatvariable.)

Function sets the maximum number of bars that is available for historical reference of a given built-in or user variable. When operator '[]' is applied to a variable - it is a reference to a historical value of that variable.
If an argument of an operator '[]' is a compile time constant value (e.g. 'v[10]', 'close[500]') then there is no need to use 'max_bars_back' function for that variable. Pine Script® compiler will use that constant value as history buffer size.
If an argument of an operator '[]' is a value, calculated at runtime (e.g. 'v[i]' where 'i' - is a series variable) then Pine Script® attempts to autodetect the history buffer size at runtime. Sometimes it fails and the script crashes at runtime because it eventually refers to historical values that are out of the buffer. In that case you should use 'max_bars_back' to fix that problem manually.
Syntax
max_bars_back(var, num) → void
Arguments
var (series int/float/bool/color/label/line) Series variable identifier for which history buffer should be resized. Possible values are: 'open', 'high', 'low', 'close', 'volume', 'time', or any user defined variable id.
num (const int) History buffer size which is the number of bars that could be referenced for variable 'var'.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("max_bars_back")
close_() => close
depth() => 400
d = depth()
v = close_()
max_bars_back(v, 500)
out = if bar_index > 0
    v[d]
else
    v
plot(out)
Returns
void
Remarks
At the moment 'max_bars_back' cannot be applied to built-ins like 'hl2', 'hlc3', 'ohlc4'. Please use multiple 'max_bars_back' calls as workaround here (e.g. instead of a single ‘max_bars_back(hl2, 100)’ call you should call the function twice: ‘max_bars_back(high, 100), max_bars_back(low, 100)’).
If the indicator() or strategy() 'max_bars_back' parameter is used, all variables in the indicator are affected. This may result in excessive memory usage and cause runtime problems. When possible (i.e. when the cause is a variable rather than a function), please use the max_bars_back() function instead.
See also
indicator()
minute()

## [Syntax](https://www.tradingview.com/pine-script-reference/v6/#fun_Syntax)

Syntax
minute(time, timezone) → series int
Arguments
time (series int) UNIX time in milliseconds.
timezone (series string) Allows adjusting the returned value to a time zone specified in either UTC/GMT notation (e.g., "UTC-5", "GMT+0530") or as an IANA time zone database name (e.g., "America/New_York"). Optional. The default is syminfo.timezone.
Returns
Minute (in exchange timezone) for provided UNIX time.
Remarks
UNIX time is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
See also
minute
time()
year()
month()
dayofmonth()
dayofweek()
hour()
second()
month()

## [Syntax](https://www.tradingview.com/pine-script-reference/v6/#fun_Syntax)

Syntax
month(time, timezone) → series int
Arguments
time (series int) UNIX time in milliseconds.
timezone (series string) Allows adjusting the returned value to a time zone specified in either UTC/GMT notation (e.g., "UTC-5", "GMT+0530") or as an IANA time zone database name (e.g., "America/New_York"). Optional. The default is syminfo.timezone.
Returns
Month (in exchange timezone) for provided UNIX time.
Remarks
UNIX time is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
Note that this function returns the month based on the time of the bar's open. For overnight sessions (e.g. EURUSD, where Monday session starts on Sunday, 17:00 UTC-4) this value can be lower by 1 than the month of the trading day.
See also
month
time()
year()
dayofmonth()
dayofweek()
hour()
minute()
second()
na()2 overloads

## [Tests if x is na.](https://www.tradingview.com/pine-script-reference/v6/#fun_Testsifxisna.)

Tests if x is na.
Syntax & Overloads
na(x) → simple bool
na(x) → series bool
Arguments
x (simple int/float) Value to be tested.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("na")
// Use the `na()` function to test for `na`.
plot(na(close[1]) ? close : close[1])
// ALTERNATIVE
// `nz()` also tests `close[1]` for `na`. It returns `close[1]` if it is not `na`, and `close` if it is.
plot(nz(close[1], close))
Returns
Returns true if x is na, false otherwise.
See also
na
fixnan()
nz()
nz()6 overloads

## [Replaces na (undefined) values with either a type-specific default value or a specified replacement.](https://www.tradingview.com/pine-script-reference/v6/#fun_Replacesnaundefinedvalueswitheitheratype-specificdefaultvalueoraspecifiedreplacement.)

Replaces na (undefined) values with either a type-specific default value or a specified replacement.
Syntax & Overloads
nz(source, replacement) → simple color
nz(source, replacement) → simple int
nz(source, replacement) → series color
nz(source, replacement) → series int
nz(source, replacement) → simple float
nz(source, replacement) → series float
Arguments
source (simple color) The source series to process.
replacement (simple color) Optional. The value the function uses to replace na values in the source series. The default depends on the source type: 0 for "int", 0.0 for "float", or #00000000 for "color".
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("nz", overlay=true)
plot(nz(ta.sma(close, 100)))
Returns
The value of source if it is not na. If the value of source is na, returns zero, or the replacement argument when one is used.
See also
na
na()
fixnan()
plot()

## [Plots a series of data on the chart.](https://www.tradingview.com/pine-script-reference/v6/#fun_Plotsaseriesofdataonthechart.)

Plots a series of data on the chart.
Syntax
plot(series, title, color, linewidth, style, trackprice, histbase, offset, join, editable, show_last, display, format, precision, force_overlay, linestyle) → plot
Arguments
series (series int/float) Series of data to be plotted. Required argument.
title (const string) Title of the plot.
color (series color) Color of the plot. You can use constants like 'color=color.red' or 'color=#ff001a' as well as complex expressions like 'color = close >= open ? color.green : color.red'. Optional argument.
linewidth (input int) Width of the plotted line. Default value is 1. Not applicable to every style.
style (input plot_style) Type of plot. Possible values are: plot.style_line, plot.style_stepline, plot.style_stepline_diamond, plot.style_histogram, plot.style_cross, plot.style_area, plot.style_columns, plot.style_circles, plot.style_linebr, plot.style_areabr, plot.style_steplinebr. Default value is plot.style_line.
trackprice (input bool) If true then a horizontal price line will be shown at the level of the last indicator value. Default is false.
histbase (input int/float) The price value used as the reference level when rendering plot with plot.style_histogram, plot.style_columns or plot.style_area style. Default is 0.0.
offset (simple int) Shifts the plot to the left or to the right on the given number of bars. Default is 0.
join (input bool) If true then plot points will be joined with line, applicable only to plot.style_cross and plot.style_circles styles. Default is false.
editable (input bool) If true then plot style will be editable in Format dialog. Default is true.
show_last (input int) Optional. The number of bars, counting backwards from the most recent bar, on which the function can draw.
display (input plot_display) Controls where the plot's information is displayed. Display options support addition and subtraction, meaning that using display.all - display.status_line will display the plot's information everywhere except in the script's status line. display.price_scale + display.status_line will display the plot only in the price scale and status line. When display arguments such as display.price_scale have user-controlled chart settings equivalents, the relevant plot information will only appear when all settings allow for it. Possible values: display.none, display.pane, display.data_window, display.price_scale, display.status_line, display.all. Optional. The default is display.all.
format (input string) Determines whether the script formats the plot's values as prices, percentages, or volume values. The argument passed to this parameter supersedes the format parameter of the indicator(), and strategy() functions. Optional. The default is the format value used by the indicator()/strategy() function. Possible values: format.price, format.percent, format.volume.
precision (input int) The number of digits after the decimal point the plot's values show on the chart pane's y-axis, the script's status line, and the Data Window. Accepts a non-negative integer less than or equal to 16. The argument passed to this parameter supersedes the precision parameter of the indicator() and strategy() functions. When the function's format parameter uses format.volume, the precision parameter will not affect the result, as the decimal precision rules defined by format.volume supersede other precision settings. Optional. The default is the precision value used by the indicator()/strategy() function.
force_overlay (const bool) If true, the plotted results will display on the main chart pane, even when the script occupies a separate pane. Optional. The default is false.
linestyle (input plot_line_style) Optional. A modifier for plot styles that display lines. It specifies whether the plotted line is solid (plot.linestyle_solid), dashed (plot.linestyle_dashed), or dotted (plot.linestyle_dotted). The modifier applies only when the function uses one of the following style arguments: plot.style_line, plot.style_linebr, plot.style_stepline, plot.style_stepline_diamond, and plot.style_area. The default is plot.linestyle_solid.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("plot")
plot(high+low, title='Title', color=color.new(#00ffaa, 70), linewidth=2, style=plot.style_area, offset=15, trackprice=true)

## [// You may fill the background between any two plots with a fill() function:](https://www.tradingview.com/pine-script-reference/v6/#fun_Youmayfillthebackgroundbetweenanytwoplotswithafillfunction)

// You may fill the background between any two plots with a fill() function:
p1 = plot(open)
p2 = plot(close)
fill(p1, p2, color=color.new(color.green, 90))
Returns
A plot object, that can be used in fill()
See also
plotshape()
plotchar()
plotarrow()
barcolor()
bgcolor()
fill()
plotarrow()

## [Plots up and down arrows on the chart. Up arrow is drawn at every indicator positive value, down arrow is drawn at every negative value. If indicator returns na then no arrow is drawn. Arrows has different height, the more absolute indicator value the longer arrow is drawn.](https://www.tradingview.com/pine-script-reference/v6/#fun_Plotsupanddownarrowsonthechart.Uparrowisdrawnateveryindicatorpositivevaluedownarrowisdrawnateverynegativevalue.Ifindicatorreturnsnathennoarrowisdrawn.Arrowshasdifferentheightthemoreabsoluteindicatorvaluethelongerarrowisdrawn.)

Plots up and down arrows on the chart. Up arrow is drawn at every indicator positive value, down arrow is drawn at every negative value. If indicator returns na then no arrow is drawn. Arrows has different height, the more absolute indicator value the longer arrow is drawn.
Syntax
plotarrow(series, title, colorup, colordown, offset, minheight, maxheight, editable, show_last, display, format, precision, force_overlay) → void
Arguments
series (series int/float) Series of data to be plotted as arrows. Required argument.
title (const string) Title of the plot.
colorup (series color) Color of the up arrows. Optional argument.
colordown (series color) Color of the down arrows. Optional argument.
offset (simple int) Shifts arrows to the left or to the right on the given number of bars. Default is 0.
minheight (input int) Minimal possible arrow height in pixels. Default is 5.
maxheight (input int) Maximum possible arrow height in pixels. Default is 100.
editable (input bool) If true then plotarrow style will be editable in Format dialog. Default is true.
show_last (input int) Optional. The number of bars, counting backwards from the most recent bar, on which the function can draw.
display (input plot_display) Controls where the plot's information is displayed. Display options support addition and subtraction, meaning that using display.all - display.status_line will display the plot's information everywhere except in the script's status line. display.price_scale + display.status_line will display the plot only in the price scale and status line. When display arguments such as display.price_scale have user-controlled chart settings equivalents, the relevant plot information will only appear when all settings allow for it. Possible values: display.none, display.pane, display.data_window, display.price_scale, display.status_line, display.all. Optional. The default is display.all.
format (input string) Determines whether the script formats the plot's values as prices, percentages, or volume values. The argument passed to this parameter supersedes the format parameter of the indicator(), and strategy() functions. Optional. The default is the format value used by the indicator()/strategy() function. Possible values: format.price, format.percent, format.volume.
precision (input int) The number of digits after the decimal point the plot's values show on the chart pane's y-axis, the script's status line, and the Data Window. Accepts a non-negative integer less than or equal to 16. The argument passed to this parameter supersedes the precision parameter of the indicator() and strategy() functions. When the function's format parameter uses format.volume, the precision parameter will not affect the result, as the decimal precision rules defined by format.volume supersede other precision settings. Optional. The default is the precision value used by the indicator()/strategy() function.
force_overlay (const bool) If true, the plotted results will display on the main chart pane, even when the script occupies a separate pane. Optional. The default is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("plotarrow example", overlay=true)
codiff = close - open
plotarrow(codiff, colorup=color.new(color.teal,40), colordown=color.new(color.orange, 40))
Remarks
Use plotarrow() function in conjunction with 'overlay=true' indicator() parameter!
See also
plot()
plotshape()
plotchar()
barcolor()
bgcolor()
plotbar()

## [Plots ohlc bars on the chart.](https://www.tradingview.com/pine-script-reference/v6/#fun_Plotsohlcbarsonthechart.)

Plots ohlc bars on the chart.
Syntax
plotbar(open, high, low, close, title, color, editable, show_last, display, format, precision, force_overlay) → void
Arguments
open (series int/float) Open series of data to be used as open values of bars. Required argument.
high (series int/float) High series of data to be used as high values of bars. Required argument.
low (series int/float) Low series of data to be used as low values of bars. Required argument.
close (series int/float) Close series of data to be used as close values of bars. Required argument.
title (const string) Title of the plotbar. Optional argument.
color (series color) Color of the ohlc bars. You can use constants like 'color=color.red' or 'color=#ff001a' as well as complex expressions like 'color = close >= open ? color.green : color.red'. Optional argument.
editable (input bool) If true then plotbar style will be editable in Format dialog. Default is true.
show_last (input int) Optional. The number of bars, counting backwards from the most recent bar, on which the function can draw.
display (input plot_display) Controls where the plot's information is displayed. Display options support addition and subtraction, meaning that using display.all - display.status_line will display the plot's information everywhere except in the script's status line. display.price_scale + display.status_line will display the plot only in the price scale and status line. When display arguments such as display.price_scale have user-controlled chart settings equivalents, the relevant plot information will only appear when all settings allow for it. Possible values: display.none, display.pane, display.data_window, display.price_scale, display.status_line, display.all. Optional. The default is display.all.
format (input string) Determines whether the script formats the plot's values as prices, percentages, or volume values. The argument passed to this parameter supersedes the format parameter of the indicator(), and strategy() functions. Optional. The default is the format value used by the indicator()/strategy() function. Possible values: format.price, format.percent, format.volume.
precision (input int) The number of digits after the decimal point the plot's values show on the chart pane's y-axis, the script's status line, and the Data Window. Accepts a non-negative integer less than or equal to 16. The argument passed to this parameter supersedes the precision parameter of the indicator() and strategy() functions. When the function's format parameter uses format.volume, the precision parameter will not affect the result, as the decimal precision rules defined by format.volume supersede other precision settings. Optional. The default is the precision value used by the indicator()/strategy() function.
force_overlay (const bool) If true, the plotted results will display on the main chart pane, even when the script occupies a separate pane. Optional. The default is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("plotbar example", overlay=true)
plotbar(open, high, low, close, title='Title', color = open < close ? color.green : color.red)
Remarks
Even if one value of open, high, low or close equal NaN then bar no draw.
The maximal value of open, high, low or close will be set as 'high', and the minimal value will be set as 'low'.
See also
plotcandle()
plotcandle()

## [Plots candles on the chart.](https://www.tradingview.com/pine-script-reference/v6/#fun_Plotscandlesonthechart.)

Plots candles on the chart.
Syntax
plotcandle(open, high, low, close, title, color, wickcolor, editable, show_last, bordercolor, display, format, precision, force_overlay) → void
Arguments
open (series int/float) Open series of data to be used as open values of candles. Required argument.
high (series int/float) High series of data to be used as high values of candles. Required argument.
low (series int/float) Low series of data to be used as low values of candles. Required argument.
close (series int/float) Close series of data to be used as close values of candles. Required argument.
title (const string) Title of the plotcandles. Optional argument.
color (series color) Color of the candles. You can use constants like 'color=color.red' or 'color=#ff001a' as well as complex expressions like 'color = close >= open ? color.green : color.red'. Optional argument.
wickcolor (series color) The color of the wick of candles. An optional argument.
editable (input bool) If true then plotcandle style will be editable in Format dialog. Default is true.
show_last (input int) Optional. The number of bars, counting backwards from the most recent bar, on which the function can draw.
bordercolor (series color) The border color of candles. An optional argument.
display (input plot_display) Controls where the plot's information is displayed. Display options support addition and subtraction, meaning that using display.all - display.status_line will display the plot's information everywhere except in the script's status line. display.price_scale + display.status_line will display the plot only in the price scale and status line. When display arguments such as display.price_scale have user-controlled chart settings equivalents, the relevant plot information will only appear when all settings allow for it. Possible values: display.none, display.pane, display.data_window, display.price_scale, display.status_line, display.all. Optional. The default is display.all.
format (input string) Determines whether the script formats the plot's values as prices, percentages, or volume values. The argument passed to this parameter supersedes the format parameter of the indicator(), and strategy() functions. Optional. The default is the format value used by the indicator()/strategy() function. Possible values: format.price, format.percent, format.volume.
precision (input int) The number of digits after the decimal point the plot's values show on the chart pane's y-axis, the script's status line, and the Data Window. Accepts a non-negative integer less than or equal to 16. The argument passed to this parameter supersedes the precision parameter of the indicator() and strategy() functions. When the function's format parameter uses format.volume, the precision parameter will not affect the result, as the decimal precision rules defined by format.volume supersede other precision settings. Optional. The default is the precision value used by the indicator()/strategy() function.
force_overlay (const bool) If true, the plotted results will display on the main chart pane, even when the script occupies a separate pane. Optional. The default is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("plotcandle example", overlay=true)
plotcandle(open, high, low, close, title='Title', color = open < close ? color.green : color.red, wickcolor=color.black)
Remarks
Even if one value of open, high, low or close equal NaN then bar no draw.
The maximal value of open, high, low or close will be set as 'high', and the minimal value will be set as 'low'.
See also
plotbar()

## [plotchar()](https://www.tradingview.com/pine-script-reference/v6/#fun_plotchar)

plotchar()

## [Plots visual shapes using any given one Unicode character on the chart.](https://www.tradingview.com/pine-script-reference/v6/#fun_PlotsvisualshapesusinganygivenoneUnicodecharacteronthechart.)

Plots visual shapes using any given one Unicode character on the chart.
Syntax
plotchar(series, title, char, location, color, offset, text, textcolor, editable, size, show_last, display, format, precision, force_overlay) → void
Arguments
series (series int/float/bool) Series of data to be plotted as shapes. Series is treated as a series of boolean values for all location values except location.absolute. Required argument.
title (const string) Title of the plot.
char (input string) Character to use as a visual shape.
location (input string) Location of shapes on the chart. Possible values are: location.abovebar, location.belowbar, location.top, location.bottom, location.absolute. Default value is location.abovebar.
color (series color) Color of the shapes. You can use constants like 'color=color.red' or 'color=#ff001a' as well as complex expressions like 'color = close >= open ? color.green : color.red'. Optional argument.
offset (simple int) Shifts shapes to the left or to the right on the given number of bars. Default is 0.
text (const string) Text to display with the shape. You can use multiline text, to separate lines use '\n' escape sequence. Example: 'line one\nline two'.
textcolor (series color) Color of the text. You can use constants like 'textcolor=color.red' or 'textcolor=#ff001a' as well as complex expressions like 'textcolor = close >= open ? color.green : color.red'. Optional argument.
editable (input bool) If true then plotchar style will be editable in Format dialog. Default is true.
size (const string) Size of characters on the chart. Possible values are: size.auto, size.tiny, size.small, size.normal, size.large, size.huge. Default is size.auto.
show_last (input int) Optional. The number of bars, counting backwards from the most recent bar, on which the function can draw.
display (input plot_display) Controls where the plot's information is displayed. Display options support addition and subtraction, meaning that using display.all - display.status_line will display the plot's information everywhere except in the script's status line. display.price_scale + display.status_line will display the plot only in the price scale and status line. When display arguments such as display.price_scale have user-controlled chart settings equivalents, the relevant plot information will only appear when all settings allow for it. Possible values: display.none, display.pane, display.data_window, display.price_scale, display.status_line, display.all. Optional. The default is display.all.
format (input string) Determines whether the script formats the plot's values as prices, percentages, or volume values. The argument passed to this parameter supersedes the format parameter of the indicator(), and strategy() functions. Optional. The default is the format value used by the indicator()/strategy() function. Possible values: format.price, format.percent, format.volume.
precision (input int) The number of digits after the decimal point the plot's values show on the chart pane's y-axis, the script's status line, and the Data Window. Accepts a non-negative integer less than or equal to 16. The argument passed to this parameter supersedes the precision parameter of the indicator() and strategy() functions. When the function's format parameter uses format.volume, the precision parameter will not affect the result, as the decimal precision rules defined by format.volume supersede other precision settings. Optional. The default is the precision value used by the indicator()/strategy() function.
force_overlay (const bool) If true, the plotted results will display on the main chart pane, even when the script occupies a separate pane. Optional. The default is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("plotchar example", overlay=true)
data = close >= open
plotchar(data, char='❄')
Remarks
Use plotchar() function in conjunction with 'overlay=true' indicator() parameter!
See also
plot()
plotshape()
plotarrow()
barcolor()
bgcolor()
plotshape()

## [Plots visual shapes on the chart.](https://www.tradingview.com/pine-script-reference/v6/#fun_Plotsvisualshapesonthechart.)

Plots visual shapes on the chart.
Syntax
plotshape(series, title, style, location, color, offset, text, textcolor, editable, size, show_last, display, format, precision, force_overlay) → void
Arguments
series (series int/float/bool) Series of data to be plotted as shapes. Series is treated as a series of boolean values for all location values except location.absolute. Required argument.
title (const string) Title of the plot.
style (input string) Type of plot. Possible values are: shape.xcross, shape.cross, shape.triangleup, shape.triangledown, shape.flag, shape.circle, shape.arrowup, shape.arrowdown, shape.labelup, shape.labeldown, shape.square, shape.diamond. Default value is shape.xcross.
location (input string) Location of shapes on the chart. Possible values are: location.abovebar, location.belowbar, location.top, location.bottom, location.absolute. Default value is location.abovebar.
color (series color) Color of the shapes. You can use constants like 'color=color.red' or 'color=#ff001a' as well as complex expressions like 'color = close >= open ? color.green : color.red'. Optional argument.
offset (simple int) Shifts shapes to the left or to the right on the given number of bars. Default is 0.
text (const string) Text to display with the shape. You can use multiline text, to separate lines use '\n' escape sequence. Example: 'line one\nline two'.
textcolor (series color) Color of the text. You can use constants like 'textcolor=color.red' or 'textcolor=#ff001a' as well as complex expressions like 'textcolor = close >= open ? color.green : color.red'. Optional argument.
editable (input bool) If true then plotshape style will be editable in Format dialog. Default is true.
size (const string) Size of shapes on the chart. Possible values are: size.auto, size.tiny, size.small, size.normal, size.large, size.huge. Default is size.auto.
show_last (input int) Optional. The number of bars, counting backwards from the most recent bar, on which the function can draw.
display (input plot_display) Controls where the plot's information is displayed. Display options support addition and subtraction, meaning that using display.all - display.status_line will display the plot's information everywhere except in the script's status line. display.price_scale + display.status_line will display the plot only in the price scale and status line. When display arguments such as display.price_scale have user-controlled chart settings equivalents, the relevant plot information will only appear when all settings allow for it. Possible values: display.none, display.pane, display.data_window, display.price_scale, display.status_line, display.all. Optional. The default is display.all.
format (input string) Determines whether the script formats the plot's values as prices, percentages, or volume values. The argument passed to this parameter supersedes the format parameter of the indicator(), and strategy() functions. Optional. The default is the format value used by the indicator()/strategy() function. Possible values: format.price, format.percent, format.volume.
precision (input int) The number of digits after the decimal point the plot's values show on the chart pane's y-axis, the script's status line, and the Data Window. Accepts a non-negative integer less than or equal to 16. The argument passed to this parameter supersedes the precision parameter of the indicator() and strategy() functions. When the function's format parameter uses format.volume, the precision parameter will not affect the result, as the decimal precision rules defined by format.volume supersede other precision settings. Optional. The default is the precision value used by the indicator()/strategy() function.
force_overlay (const bool) If true, the plotted results will display on the main chart pane, even when the script occupies a separate pane. Optional. The default is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("plotshape example 1", overlay=true)
data = close >= open
plotshape(data, style=shape.xcross)
Remarks
Use plotshape() function in conjunction with 'overlay=true' indicator() parameter!
See also
plot()
plotchar()
plotarrow()
barcolor()
bgcolor()
polyline.delete()

## [Deletes the specified polyline object. It has no effect if the id doesn't exist.](https://www.tradingview.com/pine-script-reference/v6/#fun_Deletesthespecifiedpolylineobject.Ithasnoeffectiftheiddoesntexist.)

Deletes the specified polyline object. It has no effect if the id doesn't exist.
Syntax
polyline.delete(id) → void
Arguments
id (series polyline) The polyline ID to delete.
polyline.new()

## [Creates a new polyline instance and displays it on the chart, sequentially connecting all of the points in the points array with line segments. The segments in the drawing can be straight or curved depending on the curved parameter.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createsanewpolylineinstanceanddisplaysitonthechartsequentiallyconnectingallofthepointsinthepointsarraywithlinesegments.Thesegmentsinthedrawingcanbestraightorcurveddependingonthecurvedparameter.)

Creates a new polyline instance and displays it on the chart, sequentially connecting all of the points in the points array with line segments. The segments in the drawing can be straight or curved depending on the curved parameter.
Syntax
polyline.new(points, curved, closed, xloc, line_color, fill_color, line_style, line_width, force_overlay) → series polyline
Arguments
points (array<chart.point>) An array of chart.point objects for the drawing to sequentially connect.
curved (series bool) If true, the drawing will connect all points from the points array using curved line segments. Optional. The default is false.
closed (series bool) If true, the drawing will also connect the first point to the last point from the points array, resulting in a closed polyline. Optional. The default is false.
xloc (series string) Determines the field of the chart.point objects in the points array that the polyline will use for its x-coordinates. If xloc.bar_index, the polyline will use the index field from each point. If xloc.bar_time, it will use the time field. Optional. The default is xloc.bar_index.
line_color (series color) The color of the line segments. Optional. The default is color.blue.
fill_color (series color) The fill color of the polyline. Optional. The default is na.
line_style (series string) The style of the polyline. Possible values: line.style_solid, line.style_dotted, line.style_dashed, line.style_arrow_left, line.style_arrow_right, line.style_arrow_both. Optional. The default is line.style_solid.
line_width (series int) The width of the line segments, expressed in pixels. Optional. The default is 1.
force_overlay (const bool) If true, the drawing will display on the main chart pane, even when the script occupies a separate pane. Optional. The default is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("Polylines example", overlay = true)

## [//@variable If `true`, connects all points in the polyline with curved line segments.](https://www.tradingview.com/pine-script-reference/v6/#fun_variableIftrueconnectsallpointsinthepolylinewithcurvedlinesegments.)

//@variable If `true`, connects all points in the polyline with curved line segments.
bool curvedInput = input.bool(false, "Curve Polyline")
//@variable If `true`, connects the first point in the polyline to the last point.
bool closedInput = input.bool(true, "Close Polyline")
//@variable The color of the space filled by the polyline.
color fillcolor = input.color(color.new(color.blue, 90), "Fill Color")

## [// Time and price inputs for the polyline's points.](https://www.tradingview.com/pine-script-reference/v6/#fun_Timeandpriceinputsforthepolylinespoints.)

// Time and price inputs for the polyline's points.
p1x = input.time(0,  "p1", confirm = true, inline = "p1")
p1y = input.price(0, "  ", confirm = true, inline = "p1")
p2x = input.time(0,  "p2", confirm = true, inline = "p2")
p2y = input.price(0, "  ", confirm = true, inline = "p2")
p3x = input.time(0,  "p3", confirm = true, inline = "p3")
p3y = input.price(0, "  ", confirm = true, inline = "p3")
p4x = input.time(0,  "p4", confirm = true, inline = "p4")
p4y = input.price(0, "  ", confirm = true, inline = "p4")
p5x = input.time(0,  "p5", confirm = true, inline = "p5")
p5y = input.price(0, "  ", confirm = true, inline = "p5")

## [if barstate.islastconfirmedhistory](https://www.tradingview.com/pine-script-reference/v6/#fun_ifbarstate.islastconfirmedhistory)

if barstate.islastconfirmedhistory
    //@variable An array of `chart.point` objects for the new polyline.
    var points = array.new<chart.point>()
    // Push new `chart.point` instances into the `points` array.
    points.push(chart.point.from_time(p1x, p1y))
    points.push(chart.point.from_time(p2x, p2y))
    points.push(chart.point.from_time(p3x, p3y))
    points.push(chart.point.from_time(p4x, p4y))
    points.push(chart.point.from_time(p5x, p5y))
    // Add labels for each `chart.point` in `points`.
    l1p1 = label.new(points.get(0), text = "p1", xloc = xloc.bar_time, color = na)
    l1p2 = label.new(points.get(1), text = "p2", xloc = xloc.bar_time, color = na)
    l2p1 = label.new(points.get(2), text = "p3", xloc = xloc.bar_time, color = na)
    l2p2 = label.new(points.get(3), text = "p4", xloc = xloc.bar_time, color = na)
    // Create a new polyline that connects each `chart.point` in the `points` array, starting from the first.
    polyline.new(points, curved = curvedInput, closed = closedInput, fill_color = fillcolor, xloc = xloc.bar_time)
Returns
The ID of a new polyline object that a script can use in other polyline.*() functions.
See also
chart.point.new()
request.currency_rate()

## [Provides a daily rate that can be used to convert a value expressed in the from currency to another in the to currency.](https://www.tradingview.com/pine-script-reference/v6/#fun_Providesadailyratethatcanbeusedtoconvertavalueexpressedinthefromcurrencytoanotherinthetocurrency.)

Provides a daily rate that can be used to convert a value expressed in the from currency to another in the to currency.
Syntax
request.currency_rate(from, to, ignore_invalid_currency) → series float
Arguments
from (series string) The currency in which the value to be converted is expressed. Possible values: a three-letter string with the currency code in the ISO 4217 format (e.g. "USD"), or one of the built-in variables that return currency codes, like syminfo.currency or currency.USD.
to (series string) The currency in which the value is to be converted. Possible values: a three-letter string with the currency code in the ISO 4217 format (e.g. "USD"), or one of the built-in variables that return currency codes, like syminfo.currency or currency.USD.
ignore_invalid_currency (series bool) Determines the behavior of the function if a conversion rate between the two currencies cannot be calculated: if false, the script will halt and return a runtime error; if true, the function will return na and execution will continue. Optional. The default is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("Close in British Pounds")
rate = request.currency_rate(syminfo.currency, "GBP")
plot(close * rate)
Remarks
If from and to arguments are equal, function returns 1. Please note that using this variable/function can cause indicator repainting.
request.dividends()

## [Requests dividends data for the specified symbol.](https://www.tradingview.com/pine-script-reference/v6/#fun_Requestsdividendsdataforthespecifiedsymbol.)

Requests dividends data for the specified symbol.
Syntax
request.dividends(ticker, field, gaps, lookahead, ignore_invalid_symbol, currency) → series float
Arguments
ticker (series string) Symbol. Note that the symbol should be passed with a prefix. For example: "NASDAQ:AAPL" instead of "AAPL". Using syminfo.ticker will cause an error. Use syminfo.tickerid instead.
field (series string) Input string. Possible values include: dividends.net, dividends.gross. Default value is dividends.gross.
gaps (simple barmerge_gaps) Merge strategy for the requested data (requested data automatically merges with the main series OHLC data). Possible values: barmerge.gaps_on, barmerge.gaps_off. barmerge.gaps_on - requested data is merged with possible gaps (na values). barmerge.gaps_off - requested data is merged continuously without gaps, all the gaps are filled with the previous nearest existing values. Default value is barmerge.gaps_off.
lookahead (simple barmerge_lookahead) Merge strategy for the requested data position. Possible values: barmerge.lookahead_on, barmerge.lookahead_off. Default value is barmerge.lookahead_off starting from version 3. Note that behavour is the same on real-time, and differs only on history.
ignore_invalid_symbol (input bool) An optional parameter. Determines the behavior of the function if the specified symbol is not found: if false, the script will halt and return a runtime error; if true, the function will return na and execution will continue. The default value is false.
currency (series string) Currency into which the symbol's currency-related dividends values (e.g. dividends.gross) are to be converted. The conversion rate depends on the previous daily value of a corresponding currency pair from the most popular exchange. A spread symbol is used if no exchange provides the rate directly. Possible values: a "string" representing a valid currency code (e.g., "USD" or "USDT") or a constant from the currency.* namespace (e.g., currency.USD or currency.USDT). The default is syminfo.currency.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("request.dividends")
s1 = request.dividends("NASDAQ:BELFA")
plot(s1)
s2 = request.dividends("NASDAQ:BELFA", dividends.net, gaps=barmerge.gaps_on, lookahead=barmerge.lookahead_on)
plot(s2)
Returns
Requested series, or n/a if there is no dividends data for the specified symbol.
See also
request.earnings()
request.splits()
request.security()
syminfo.tickerid
request.earnings()

## [Requests earnings data for the specified symbol.](https://www.tradingview.com/pine-script-reference/v6/#fun_Requestsearningsdataforthespecifiedsymbol.)

Requests earnings data for the specified symbol.
Syntax
request.earnings(ticker, field, gaps, lookahead, ignore_invalid_symbol, currency) → series float
Arguments
ticker (series string) Symbol. Note that the symbol should be passed with a prefix. For example: "NASDAQ:AAPL" instead of "AAPL". Using syminfo.ticker will cause an error. Use syminfo.tickerid instead.
field (series string) Input string. Possible values include: earnings.actual, earnings.estimate, earnings.standardized. Default value is earnings.actual.
gaps (simple barmerge_gaps) Merge strategy for the requested data (requested data automatically merges with the main series OHLC data). Possible values: barmerge.gaps_on, barmerge.gaps_off. barmerge.gaps_on - requested data is merged with possible gaps (na values). barmerge.gaps_off - requested data is merged continuously without gaps, all the gaps are filled with the previous nearest existing values. Default value is barmerge.gaps_off.
lookahead (simple barmerge_lookahead) Merge strategy for the requested data position. Possible values: barmerge.lookahead_on, barmerge.lookahead_off. Default value is barmerge.lookahead_off starting from version 3. Note that behavour is the same on real-time, and differs only on history.
ignore_invalid_symbol (input bool) An optional parameter. Determines the behavior of the function if the specified symbol is not found: if false, the script will halt and return a runtime error; if true, the function will return na and execution will continue. The default value is false.
currency (series string) Currency into which the symbol's currency-related earnings values (e.g. earnings.actual) are to be converted. The conversion rate depends on the previous daily value of a corresponding currency pair from the most popular exchange. A spread symbol is used if no exchange provides the rate directly. Possible values: a "string" representing a valid currency code (e.g., "USD" or "USDT") or a constant from the currency.* namespace (e.g., currency.USD or currency.USDT). The default is syminfo.currency.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("request.earnings")
s1 = request.earnings("NASDAQ:BELFA")
plot(s1)
s2 = request.earnings("NASDAQ:BELFA", earnings.actual, gaps=barmerge.gaps_on, lookahead=barmerge.lookahead_on)
plot(s2)
Returns
Requested series, or n/a if there is no earnings data for the specified symbol.
See also
request.dividends()
request.splits()
request.security()
syminfo.tickerid
request.economic()

## [Requests economic data for a symbol. Economic data includes information such as the state of a country's economy (GDP, inflation rate, etc.) or of a particular industry (steel production, ICU beds, etc.).](https://www.tradingview.com/pine-script-reference/v6/#fun_Requestseconomicdataforasymbol.EconomicdataincludesinformationsuchasthestateofacountryseconomyGDPinflationrateetc.orofaparticularindustrysteelproductionICUbedsetc..)

Requests economic data for a symbol. Economic data includes information such as the state of a country's economy (GDP, inflation rate, etc.) or of a particular industry (steel production, ICU beds, etc.).
Syntax
request.economic(country_code, field, gaps, ignore_invalid_symbol) → series float
Arguments
country_code (series string) The code of the country (e.g. "US") or the region (e.g. "EU") for which the economic data is requested. The Help Center article lists the countries and their codes. The countries for which information is available vary with metrics. The Help Center article for each metric lists the countries for which the metric is available.
field (series string) The code of the requested economic metric (e.g., "GDP"). The Help Center article lists the metrics and their codes.
gaps (simple barmerge_gaps) Specifies how the returned values are merged on chart bars. Possible values: barmerge.gaps_off, barmerge.gaps_on. With barmerge.gaps_on, a value only appears on the current chart bar when it first becomes available from the function's context, otherwise na is returned (thus a "gap" occurs). With barmerge.gaps_off, what would otherwise be gaps are filled with the latest known value returned, avoiding na values. Optional. The default is barmerge.gaps_off.
ignore_invalid_symbol (input bool) Determines the behavior of the function if the specified symbol is not found: if false, the script will halt and return a runtime error; if true, the function will return na and execution will continue. Optional. The default is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("US GDP")
e = request.economic("US", "GDP")
plot(e)
Returns
Requested series.
Remarks
Economic data can also be accessed from charts, just like a regular symbol. Use "ECONOMIC" as the exchange name and {country_code}{field} as the ticker. The name of US GDP data is thus "ECONOMIC:USGDP".
See also
request.financial()
request.financial()

## [Requests financial series for symbol.](https://www.tradingview.com/pine-script-reference/v6/#fun_Requestsfinancialseriesforsymbol.)

Requests financial series for symbol.
Syntax
request.financial(symbol, financial_id, period, gaps, ignore_invalid_symbol, currency) → series float
Arguments
symbol (series string) Symbol. Note that the symbol should be passed with a prefix. For example: "NASDAQ:AAPL" instead of "AAPL".
financial_id (series string) Financial identifier. You can find the list of available ids via our Help Center.
period (series string) Reporting period. Possible values are "TTM", "FY", "FQ", "FH", "D".
gaps (simple barmerge_gaps) Merge strategy for the requested data (requested data automatically merges with the main series: OHLC data). Possible values include: barmerge.gaps_on, barmerge.gaps_off. barmerge.gaps_on - requested data is merged with possible gaps (na values). barmerge.gaps_off - requested data is merged continuously without gaps, all the gaps are filled with the previous, nearest existing values. Default value is barmerge.gaps_off.
ignore_invalid_symbol (input bool) An optional parameter. Determines the behavior of the function if the specified symbol is not found: if false, the script will halt and return a runtime error; if true, the function will return na and execution will continue. The default value is false.
currency (series string) Optional. Currency into which the symbol's financial metrics (e.g. Net Income) are to be converted. The conversion rate depends on the previous daily value of a corresponding currency pair from the most popular exchange. A spread symbol is used if no exchange provides the rate directly. Possible values: a "string" representing a valid currency code (e.g., "USD" or "USDT") or a constant from the currency.* namespace (e.g., currency.USD or currency.USDT). The default is syminfo.currency.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("request.financial")
f = request.financial("NASDAQ:MSFT", "ACCOUNTS_PAYABLE", "FY")
plot(f)
Returns
Requested series.
See also
request.security()
syminfo.tickerid
request.footprint()

## [Requests the ID of a footprint object that contains data for calculating volume footprint information for the current chart bar. Scripts can use the returned ID in calls to the footprint.*() functions to retrieve footprint data, including footprint rows, categorized volume sums, and volume delta.](https://www.tradingview.com/pine-script-reference/v6/#fun_RequeststheIDofafootprintobjectthatcontainsdataforcalculatingvolumefootprintinformationforthecurrentchartbar.ScriptscanusethereturnedIDincallstothefootprint.functionstoretrievefootprintdataincludingfootprintrowscategorizedvolumesumsandvolumedelta.)

Requests the ID of a footprint object that contains data for calculating volume footprint information for the current chart bar. Scripts can use the returned ID in calls to the footprint.*() functions to retrieve footprint data, including footprint rows, categorized volume sums, and volume delta.
Syntax
request.footprint(ticks_per_row, va_percent, imbalance_percent) → footprint
Arguments
ticks_per_row (simple int) The price range of each footprint row, expressed in ticks.
va_percent (simple int/float) Optional. The percentage of each footprint's total volume to use for calculating the value area (VA). The default is 70.
imbalance_percent (simple int/float) Optional. The percentage difference in volume for detecting row imbalances. Scripts can use volume_row IDs retrieved from the returned footprint object in calls to volume_row.has_buy_imbalance() and volume_row.has_sell_imbalance() to identify imbalanced rows. A row is imbalanced if its "buy" volume exceeds the "sell" volume of the row below it by the specified percentage, or if its "sell" volume exceeds the "buy" volume of the row above it by the percentage. The default is 300.
Returns
The ID of a footprint object containing volume footprint data for the current bar, or na if no data is available.
Remarks
Only accounts with Premium or Ultimate plans can use scripts that call this function.
A single script cannot include more than one request.footprint() call.
request.quandl()

## [Note: This function has been deprecated due to the API change from NASDAQ Data Link. Requests for "QUANDL" symbols are no longer valid and requests for them return a runtime error.](https://www.tradingview.com/pine-script-reference/v6/#fun_NoteThisfunctionhasbeendeprecatedduetotheAPIchangefromNASDAQDataLink.RequestsforQUANDLsymbolsarenolongervalidandrequestsforthemreturnaruntimeerror.)

Note: This function has been deprecated due to the API change from NASDAQ Data Link. Requests for "QUANDL" symbols are no longer valid and requests for them return a runtime error.
Some of the data previously provided by this function is available on TradingView through other feeds, such as "BCHAIN" or "FRED". Use Symbol Search to look for such data based on its description. Commitment of Traders (COT) data can be requested using the official LibraryCOT library.
Requests Nasdaq Data Link (formerly Quandl) data for a symbol.
Syntax
request.quandl(ticker, gaps, index, ignore_invalid_symbol) → series float
Arguments
ticker (series string) Symbol. Note that the name of a time series and Quandl data feed should be divided by a forward slash. For example: "CFTC/SB_FO_ALL".
gaps (simple barmerge_gaps) Merge strategy for the requested data (requested data automatically merges with the main series: OHLC data). Possible values include: barmerge.gaps_on, barmerge.gaps_off. barmerge.gaps_on - requested data is merged with possible gaps (na values). barmerge.gaps_off - requested data is merged continuously without gaps, all the gaps are filled with the previous, nearest existing values. Default value is barmerge.gaps_off.
index (series int) A Quandl time-series column index.
ignore_invalid_symbol (input bool) An optional parameter. Determines the behavior of the function if the specified symbol is not found: if false, the script will halt and return a runtime error; if true, the function will return na and execution will continue. The default value is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("request.quandl")
f = request.quandl("CFTC/SB_FO_ALL", barmerge.gaps_off, 0)
plot(f)
Returns
Requested series.
See also
request.security()
syminfo.tickerid
request.security()

## [Requests the result of an expression from a specified context (symbol and timeframe).](https://www.tradingview.com/pine-script-reference/v6/#fun_Requeststheresultofanexpressionfromaspecifiedcontextsymbolandtimeframe.)

Requests the result of an expression from a specified context (symbol and timeframe).
Syntax
request.security(symbol, timeframe, expression, gaps, lookahead, ignore_invalid_symbol, currency, calc_bars_count) → series <type>
Arguments
symbol (series string) Symbol or ticker identifier of the requested data. Use an empty string or syminfo.tickerid to request data using the chart's symbol. To retrieve data with additional modifiers (extended sessions, dividend adjustments, non-standard chart types like Heikin Ashi and Renko, etc.), create a custom ticker ID for the request using the functions in the ticker.*namespace.
timeframe (series string) Timeframe of the requested data. Use an empty string or timeframe.period to request data from the chart's timeframe or the timeframe specified in the indicator() function. To request data from a different timeframe, supply a valid timeframe string. See here to learn about specifying timeframe strings.
expression (variable, function, object, array, matrix, or map of series int/float/bool/string/color/enum, or a tuple of these) The expression to calculate and return from the requested context. It can accept a built-in variable like close, a user-defined variable, an expression such as ta.change(close) / (high - low), a function call that does not use Pine Script® drawings, an object, a collection, or a tuple of expressions.
gaps (simple barmerge_gaps) Specifies how the returned values are merged on chart bars. Possible values: barmerge.gaps_on, barmerge.gaps_off. With barmerge.gaps_on a value only appears on the current chart bar when it first becomes available from the function's context, otherwise na is returned (thus a "gap" occurs). With barmerge.gaps_off what would otherwise be gaps are filled with the latest known value returned, avoiding na values. Optional. The default is barmerge.gaps_off.
lookahead (simple barmerge_lookahead) On historical bars only, returns data from the timeframe before it elapses. Possible values: barmerge.lookahead_on, barmerge.lookahead_off. Has no effect on realtime values. Optional. The default is barmerge.lookahead_off starting from Pine Script® v3. The default is barmerge.lookahead_on in v1 and v2. WARNING: Using barmerge.lookahead_on at timeframes higher than the chart's without offsetting the expression argument like in close[1] will introduce future leak in scripts, as the function will then return the close price before it is actually known in the current context. As is explained in the User Manual's page on Repainting this will produce misleading results.
ignore_invalid_symbol (input bool) Determines the behavior of the function if the specified symbol is not found: if false, the script will halt and throw a runtime error; if true, the function will return na and execution will continue. Optional. The default is false.
currency (series string) Optional. Specifies the target currency for converting values expressed in currency units (e.g., open, high, low, close) or expressions involving such values. Literal values such as 200 are not converted. The conversion rate for monetary values depends on the previous daily value of a corresponding currency pair from the most popular exchange. A spread symbol is used if no exchange provides the rate directly. Possible values: a "string" representing a valid currency code (e.g., "USD" or "USDT") or a constant from the currency.* namespace (e.g., currency.USD or currency.USDT). The default is syminfo.currency.
calc_bars_count (simple int) Optional. Determines the maximum number of recent historical bars that the function can request. If specified, the function evaluates the expression argument starting from that number of bars behind the last historical bar in the requested dataset, treating those bars as the only available data. Limiting the number of historical bars in a request can help improve calculation efficiency in some cases. The default is the same as the number of chart bars available for the symbol and timeframe. The maximum number of bars that the function can attempt to retrieve depends on the intrabar limit of the user's plan. However, the request cannot retrieve more bars than are available in the dataset.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("Simple `request.security()` calls")
// Returns 1D close of the current symbol.
dailyClose = request.security(syminfo.tickerid, "1D", close)
plot(dailyClose)

## [// Returns the close of "AAPL" from the same timeframe as currently open on the chart.](https://www.tradingview.com/pine-script-reference/v6/#fun_ReturnsthecloseofAAPLfromthesametimeframeascurrentlyopenonthechart.)

// Returns the close of "AAPL" from the same timeframe as currently open on the chart.
aaplClose = request.security("AAPL", timeframe.period, close)
plot(aaplClose)
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("Advanced `request.security()` calls")
// This calculates a 10-period moving average on the active chart.
sma = ta.sma(close, 10)
// This sends the `sma` calculation for execution in the context of the "AAPL" symbol at a "240" (4 hours) timeframe.
aaplSma = request.security("AAPL", "240", sma)
plot(aaplSma)

## [// To avoid differences on historical and realtime bars, you can use this technique, which only returns a value from the higher timeframe on the bar after it completes:](https://www.tradingview.com/pine-script-reference/v6/#fun_Toavoiddifferencesonhistoricalandrealtimebarsyoucanusethistechniquewhichonlyreturnsavaluefromthehighertimeframeonthebarafteritcompletes)

// To avoid differences on historical and realtime bars, you can use this technique, which only returns a value from the higher timeframe on the bar after it completes:
indexHighTF = barstate.isrealtime ? 1 : 0
indexCurrTF = barstate.isrealtime ? 0 : 1
nonRepaintingClose = request.security[syminfo.tickerid, "1D", close[indexHighTF]](indexCurrTF)
plot(nonRepaintingClose, "Non-repainting close")

## [// Returns the 1H close of "AAPL", extended session included. The value is dividend-adjusted.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthe1HcloseofAAPLextendedsessionincluded.Thevalueisdividend-adjusted.)

// Returns the 1H close of "AAPL", extended session included. The value is dividend-adjusted.
extendedTicker = ticker.modify("NASDAQ:AAPL", session = session.extended, adjustment = adjustment.dividends)
aaplExtAdj = request.security(extendedTicker, "60", close)
plot(aaplExtAdj)

## [// Returns the result of a user-defined function.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnstheresultofauser-definedfunction.)

// Returns the result of a user-defined function.
// The `max` variable is mutable, but we can pass it to `request.security()` because it is wrapped in a function.
allTimeHigh(source) =>
    var max = source
    max := math.max(max, source)
allTimeHigh1D = request.security(syminfo.tickerid, "1D", allTimeHigh(high))

## [// By using a tuple `expression`, we obtain several values with only one `request.security()` call.](https://www.tradingview.com/pine-script-reference/v6/#fun_Byusingatupleexpressionweobtainseveralvalueswithonlyonerequest.securitycall.)

// By using a tuple `expression`, we obtain several values with only one `request.security()` call.
[open1D, high1D, low1D, close1D, ema1D] = request.security(syminfo.tickerid, "1D", [open, high, low, close, ta.ema(close, 10)])
plotcandle(open1D, high1D, low1D, close1D)
plot(ema1D)

## [// Returns an array containing the OHLC values of the chart's symbol from the 1D timeframe.](https://www.tradingview.com/pine-script-reference/v6/#fun_ReturnsanarraycontainingtheOHLCvaluesofthechartssymbolfromthe1Dtimeframe.)

// Returns an array containing the OHLC values of the chart's symbol from the 1D timeframe.
ohlcArray = request.security(syminfo.tickerid, "1D", array.from(open, high, low, close))
plotcandle(array.get(ohlcArray, 0), array.get(ohlcArray, 1), array.get(ohlcArray, 2), array.get(ohlcArray, 3))
Returns
A result determined by expression.
Remarks
Scripts using this function might calculate differently on historical and realtime bars, leading to repainting.
A single script can contain no more than 40 unique request.*() function calls. A call is unique only if it does not call the same function with the same arguments.
When using two calls to a request.*() function to evaluate the same expression from the same context with different calc_bars_count values, the second call requests the same number of historical bars as the first. For example, if a script calls request.security("AAPL", "", close, calc_bars_count = 3) after it calls request.security("AAPL", "", close, calc_bars_count = 5), the second call also uses five bars of historical data, not three.
The symbol of a request.() call can be inherited if it is not specified precisely, i.e., if the symbol argument is an empty string or syminfo.tickerid. Similarly, the timeframe of a request.() call can be inherited if the timeframe argument is an empty string or timeframe.period. These values are normally taken from the chart on which the script is running. However, if request.*() function A is called from within the expression of request.*() function B, then function A can inherit the values from function B. See here for more information.
See also
syminfo.ticker
syminfo.tickerid
timeframe.period
ticker.new()
ticker.modify()
request.security_lower_tf()
request.dividends()
request.earnings()
request.splits()
request.financial()
request.security_lower_tf()

## [Requests the results of an expression from a specified symbol on a timeframe lower than or equal to the chart's timeframe. It returns an array containing one element for each lower-timeframe bar within the chart bar. On a 5-minute chart, requesting data using a timeframe argument of "1" typically returns an array with five elements representing the value of the expression on each 1-minute bar, ordered by time with the earliest value first.](https://www.tradingview.com/pine-script-reference/v6/#fun_Requeststheresultsofanexpressionfromaspecifiedsymbolonatimeframelowerthanorequaltothechartstimeframe.Itreturnsanarraycontainingoneelementforeachlower-timeframebarwithinthechartbar.Ona5-minutechartrequestingdatausingatimeframeargumentof1typicallyreturnsanarraywithfiveelementsrepresentingthevalueoftheexpressiononeach1-minutebarorderedbytimewiththeearliestvaluefirst.)

Requests the results of an expression from a specified symbol on a timeframe lower than or equal to the chart's timeframe. It returns an array containing one element for each lower-timeframe bar within the chart bar. On a 5-minute chart, requesting data using a timeframe argument of "1" typically returns an array with five elements representing the value of the expression on each 1-minute bar, ordered by time with the earliest value first.
Syntax
request.security_lower_tf(symbol, timeframe, expression, ignore_invalid_symbol, currency, ignore_invalid_timeframe, calc_bars_count) → array<type>
Arguments
symbol (series string) Symbol or ticker identifier of the requested data. Use an empty string or syminfo.tickerid to request data using the chart's symbol. To retrieve data with additional modifiers (extended sessions, dividend adjustments, non-standard chart types like Heikin Ashi and Renko, etc.), create a custom ticker ID for the request using the functions in the ticker.*namespace.
timeframe (series string) Timeframe of the requested data. Use an empty string or timeframe.period to request data from the chart's timeframe or the timeframe specified in the indicator() function. To request data from a different timeframe, supply a valid timeframe string. See here to learn about specifying timeframe strings.
expression (variable, object or function of series int/float/bool/string/color/enum, or a tuple of these) The expression to calculate and return from the requested context. It can accept a built-in variable like close, a user-defined variable, an expression such as ta.change(close) / (high - low), a function call that does not use Pine Script® drawings, an object, or a tuple of expressions. Collections are not allowed unless they are within the fields of an object
ignore_invalid_symbol (series bool) Determines the behavior of the function if the specified symbol is not found: if false, the script will halt and throw a runtime error; if true, the function will return na and execution will continue. Optional. The default is false.
currency (series string) Optional. Specifies the target currency for converting values expressed in currency units (e.g., open, high, low, close) or expressions involving such values. Literal values such as 200 are not converted. The conversion rate for monetary values depends on the previous daily value of a corresponding currency pair from the most popular exchange. A spread symbol is used if no exchange provides the rate directly. Possible values: a "string" representing a valid currency code (e.g., "USD" or "USDT") or a constant from the currency.* namespace (e.g., currency.USD or currency.USDT). The default is syminfo.currency.
ignore_invalid_timeframe (series bool) Determines the behavior of the function when the chart's timeframe is smaller than the timeframe used in the function call. If false, the script will halt and throw a runtime error. If true, the function will return na and execution will continue. Optional. The default is false.
calc_bars_count (simple int) Optional. Determines the maximum number of recent historical bars that the function can request. If specified, the function evaluates the expression argument starting from that number of bars behind the last historical bar in the requested dataset, treating those bars as the only available data. Limiting the number of historical bars in a request can help improve calculation efficiency in some cases. The default is the same as the number of chart bars available for the symbol and timeframe. The maximum number of bars that the function can attempt to retrieve depends on the intrabar limit of the user's plan. However, the request cannot retrieve more bars than are available in the dataset.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("`request.security_lower_tf()` Example", overlay = true)

## [// If the current chart timeframe is set to 120 minutes, then the `arrayClose` array will contain two 'close' values from the 60 minute timeframe for each bar.](https://www.tradingview.com/pine-script-reference/v6/#fun_Ifthecurrentcharttimeframeissetto120minutesthenthearrayClosearraywillcontaintwoclosevaluesfromthe60minutetimeframeforeachbar.)

// If the current chart timeframe is set to 120 minutes, then the `arrayClose` array will contain two 'close' values from the 60 minute timeframe for each bar.
arrClose = request.security_lower_tf(syminfo.tickerid, "60", close)

## [if bar_index == last_bar_index - 1](https://www.tradingview.com/pine-script-reference/v6/#fun_ifbar_indexlast_bar_index-1)

if bar_index == last_bar_index - 1
    label.new(bar_index, high, str.tostring(arrClose))
Returns
An array of a type determined by expression, or a tuple of these.
Remarks
Scripts using this function might calculate differently on historical and realtime bars, leading to repainting.
Please note that spreads (e.g., "AAPL+MSFT*TSLA") do not always return reliable data with this function.
A single script can contain no more than 40 unique request.*() function calls. A call is unique only if it does not call the same function with the same arguments.
When using two calls to a request.*() function to evaluate the same expression from the same context with different calc_bars_count values, the second call requests the same number of historical bars as the first. For example, if a script calls request.security("AAPL", "", close, calc_bars_count = 3) after it calls request.security("AAPL", "", close, calc_bars_count = 5), the second call also uses five bars of historical data, not three.
The symbol of a request.() call can be inherited if it is not specified precisely, i.e., if the symbol argument is an empty string or syminfo.tickerid. Similarly, the timeframe of a request.() call can be inherited if the timeframe argument is an empty string or timeframe.period. These values are normally taken from the chart that the script is running on. However, if request.*() function A is called from within the expression of request.*() function B, then function A can inherit the values from function B. See here for more information.
See also
request.security()
syminfo.ticker
syminfo.tickerid
timeframe.period
ticker.new()
request.dividends()
request.earnings()
request.splits()
request.financial()
request.seed()

## [Requests the result of an expression evaluated on data from a user-maintained GitHub repository. **Note:**The creation of new Pine Seeds repositories is suspended; only existing repositories are currently supported. See the Pine Seeds documentation on GitHub to learn more.](https://www.tradingview.com/pine-script-reference/v6/#fun_Requeststheresultofanexpressionevaluatedondatafromauser-maintainedGitHubrepository.NoteThecreationofnewPineSeedsrepositoriesissuspendedonlyexistingrepositoriesarecurrentlysupported.SeethePineSeedsdocumentationonGitHubtolearnmore.)

Requests the result of an expression evaluated on data from a user-maintained GitHub repository. **Note:**The creation of new Pine Seeds repositories is suspended; only existing repositories are currently supported. See the Pine Seeds documentation on GitHub to learn more.
Syntax
request.seed(source, symbol, expression, ignore_invalid_symbol, calc_bars_count) → series <type>
Arguments
source (series string) Name of the GitHub repository.
symbol (series string) Name of the file in the GitHub repository containing the data. The ".csv" file extension must not be included.
expression (<arg_expr_type>) An expression to be calculated and returned from the requested symbol's context. It can be a built-in variable like close, an expression such as ta.sma(close, 100), a non-mutable variable previously calculated in the script, a function call that does not use Pine Script® drawings, an array, a matrix, or a tuple. Mutable variables are not allowed, unless they are enclosed in the body of a function used in the expression.
ignore_invalid_symbol (input bool) Determines the behavior of the function if the specified symbol is not found: if false, the script will halt and throw a runtime error; if true, the function will return na and execution will continue. Optional. The default is false.
calc_bars_count (simple int) Optional. If specified, the function requests only this number of values from the end of the symbol's history and calculates expression as if these values are the only available data, which might improve calculation speed in some cases. The default is the same as the number of chart bars available for the symbol and timeframe. The maximum number of bars that the function can attempt to retrieve depends on the intrabar limit of the user's plan. However, the request cannot retrieve more bars than are available in the dataset.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("BTC Development Activity")

## [[devAct, devActSMA] = request.seed("seed_crypto_santiment", "BTC_DEV_ACTIVITY", [close, ta.sma(close, 10)])](https://www.tradingview.com/pine-script-reference/v6/#fun_devActdevActSMArequest.seedseed_crypto_santimentBTC_DEV_ACTIVITYcloseta.smaclose10)

[devAct, devActSMA] = request.seed("seed_crypto_santiment", "BTC_DEV_ACTIVITY", [close, ta.sma(close, 10)])

## [plot(devAct, "BTC Development Activity")](https://www.tradingview.com/pine-script-reference/v6/#fun_plotdevActBTCDevelopmentActivity)

plot(devAct, "BTC Development Activity")
plot(devActSMA, "BTC Development Activity SMA10", color = color.yellow)
Returns
Requested series or tuple of series, which may include array/matrix IDs.
request.splits()

## [Requests splits data for the specified symbol.](https://www.tradingview.com/pine-script-reference/v6/#fun_Requestssplitsdataforthespecifiedsymbol.)

Requests splits data for the specified symbol.
Syntax
request.splits(ticker, field, gaps, lookahead, ignore_invalid_symbol) → series float
Arguments
ticker (series string) Symbol. Note that the symbol should be passed with a prefix. For example: "NASDAQ:AAPL" instead of "AAPL". Using syminfo.ticker will cause an error. Use syminfo.tickerid instead.
field (series string) Input string. Possible values include: splits.denominator, splits.numerator.
gaps (simple barmerge_gaps) Merge strategy for the requested data (requested data automatically merges with the main series OHLC data). Possible values: barmerge.gaps_on, barmerge.gaps_off. barmerge.gaps_on - requested data is merged with possible gaps (na values). barmerge.gaps_off - requested data is merged continuously without gaps, all the gaps are filled with the previous nearest existing values. Default value is barmerge.gaps_off.
lookahead (simple barmerge_lookahead) Merge strategy for the requested data position. Possible values: barmerge.lookahead_on, barmerge.lookahead_off. Default value is barmerge.lookahead_off starting from version 3. Note that behavour is the same on real-time, and differs only on history.
ignore_invalid_symbol (input bool) An optional parameter. Determines the behavior of the function if the specified symbol is not found: if false, the script will halt and return a runtime error; if true, the function will return na and execution will continue. The default value is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("request.splits")
s1 = request.splits("NASDAQ:BELFA", splits.denominator)
plot(s1)
s2 = request.splits("NASDAQ:BELFA", splits.denominator, gaps=barmerge.gaps_on, lookahead=barmerge.lookahead_on)
plot(s2)
Returns
Requested series, or n/a if there is no splits data for the specified symbol.
See also
request.earnings()
request.dividends()
request.security()
syminfo.tickerid
runtime.error()

## [When called, causes a runtime error with the error message specified in the message argument.](https://www.tradingview.com/pine-script-reference/v6/#fun_Whencalledcausesaruntimeerrorwiththeerrormessagespecifiedinthemessageargument.)

When called, causes a runtime error with the error message specified in the message argument.
Syntax
runtime.error(message) → void
Arguments
message (series string) Error message.
second()

## [Syntax](https://www.tradingview.com/pine-script-reference/v6/#fun_Syntax)

Syntax
second(time, timezone) → series int
Arguments
time (series int) UNIX time in milliseconds.
timezone (series string) Allows adjusting the returned value to a time zone specified in either UTC/GMT notation (e.g., "UTC-5", "GMT+0530") or as an IANA time zone database name (e.g., "America/New_York"). Optional. The default is syminfo.timezone.
Returns
Second (in exchange timezone) for provided UNIX time.
Remarks
UNIX time is the number of milliseconds that have elapsed since 00:00:00 UTC, 1 January 1970.
See also
second
time()
year()
month()
dayofmonth()
dayofweek()
hour()
minute()
str.contains()3 overloads

## [Returns true if the source string contains the str substring, false otherwise.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnstrueifthesourcestringcontainsthestrsubstringfalseotherwise.)

Returns true if the source string contains the str substring, false otherwise.
Syntax & Overloads
str.contains(source, str) → const bool
str.contains(source, str) → simple bool
str.contains(source, str) → series bool
Arguments
source (const string) Source string.
str (const string) The substring to search for.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("str.contains")
// If the current chart is a continuous futures chart, e.g “BTC1!”, then the function will return true, false otherwise.
var isFutures = str.contains(syminfo.tickerid, "!")
plot(isFutures ? 1 : 0)
Returns
True if the str was found in the source string, false otherwise.
See also
str.pos()
str.match()
str.endswith()3 overloads

## [Returns true if the source string ends with the substring specified in str, false otherwise.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnstrueifthesourcestringendswiththesubstringspecifiedinstrfalseotherwise.)

Returns true if the source string ends with the substring specified in str, false otherwise.
Syntax & Overloads
str.endswith(source, str) → const bool
str.endswith(source, str) → simple bool
str.endswith(source, str) → series bool
Arguments
source (const string) Source string.
str (const string) The substring to search for.
Returns
True if the source string ends with the substring specified in str, false otherwise.
See also
str.startswith()
str.format()2 overloads

## [Creates a formatted string using a specified formatting string (formatString) and one or more additional arguments (arg0, arg1, etc.). The formatting string defines the structure of the returned string, where all placeholders in curly brackets ({}) refer to the additional arguments. Each placeholder requires a number representing an argument's position, starting from 0. For instance, the placeholder {0} refers to the first argument after formatString (arg0), {1} refers to the second (arg1), and so on. The function replaces each placeholder with a string representation of the corresponding argument.](https://www.tradingview.com/pine-script-reference/v6/#fun_CreatesaformattedstringusingaspecifiedformattingstringformatStringandoneormoreadditionalargumentsarg0arg1etc..Theformattingstringdefinesthestructureofthereturnedstringwhereallplaceholdersincurlybracketsrefertotheadditionalarguments.Eachplaceholderrequiresanumberrepresentinganargumentspositionstartingfrom0.Forinstancetheplaceholder0referstothefirstargumentafterformatStringarg01referstothesecondarg1andsoon.Thefunctionreplaceseachplaceholderwithastringrepresentationofthecorrespondingargument.)

Creates a formatted string using a specified formatting string (formatString) and one or more additional arguments (arg0, arg1, etc.). The formatting string defines the structure of the returned string, where all placeholders in curly brackets ({}) refer to the additional arguments. Each placeholder requires a number representing an argument's position, starting from 0. For instance, the placeholder {0} refers to the first argument after formatString (arg0), {1} refers to the second (arg1), and so on. The function replaces each placeholder with a string representation of the corresponding argument.
Syntax & Overloads
str.format(formatString, arg0, arg1, ...) → simple string
str.format(formatString, arg0, arg1, ...) → series string
Arguments
formatString (simple string) Format string.
arg0, arg1, ... (simple int/float/bool/string) Values to format.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("Simple `str.format()` demo")

## [//@variable A formatted string that includes representations of the current `bar_index` and `close` values.](https://www.tradingview.com/pine-script-reference/v6/#fun_variableAformattedstringthatincludesrepresentationsofthecurrentbar_indexandclosevalues.)

//@variable A formatted string that includes representations of the current `bar_index` and `close` values.
//          The placeholder `{0}` refers to the first argument after the formatting string (`bar_index`), and
//          `{1}` refers to the second (`close`).
string labelText = str.format("Current bar index: {0}\nCurrent bar close: {1}", bar_index, close)

## [// Draw a label to display the `labelText` string at the current bar's `high` price.](https://www.tradingview.com/pine-script-reference/v6/#fun_DrawalabeltodisplaythelabelTextstringatthecurrentbarshighprice.)

// Draw a label to display the `labelText` string at the current bar's `high` price.
label.new(bar_index, high, labelText)
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("Extensive `str.format()` demo", overlay=true)
// The format specifier inside the curly braces accepts certain modifiers:
// - Specify the number of decimals to display:
s1 = str.format("{0,number,#.#}", 1.34) // returns: 1.3
label.new(bar_index, close, text=s1)
// - Round a float value to an integer:
s2 = str.format("{0,number,integer}", 1.34) // returns: 1
label.new(bar_index - 1, close, text=s2)
// - Display a number in currency:
s3 = str.format("{0,number,currency}", 1.34) // returns: $1.34
label.new(bar_index - 2, close, text=s3)
// - Display a number as a percentage:
s4 = str.format("{0,number,percent}", 0.5) // returns: 50%
label.new(bar_index - 3, close, text=s4)
// EXAMPLES WITH SEVERAL ARGUMENTS
// returns: Number 1 is not equal to 4
s5 = str.format("Number {0} is not {1} to {2}", 1, "equal", 4)
label.new(bar_index - 4, close, text=s5)
// returns: 1.34 != 1.3
s6 = str.format("{0} != {0, number, #.#}", 1.34)
label.new(bar_index - 5, close, text=s6)
// returns: 1 is equal to 1, but 2 is equal to 2
s7 = str.format("{0, number, integer} is equal to 1, but {1, number, integer} is equal to 2", 1.34, 1.52)
label.new(bar_index - 6, close, text=s7)
// returns: The cash turnover amounted to $1,340,000.00
s8 = str.format("The cash turnover amounted to {0, number, currency}", 1340000)
label.new(bar_index - 7, close, text=s8)
// returns: Expected return is 10% - 20%
s9 = str.format("Expected return is {0, number, percent} - {1, number, percent}", 0.1, 0.2)
label.new(bar_index - 8, close, text=s9)
Returns
The formatted string.
Remarks
The string used as the formatString argument can contain single quote characters ('). However, programmers must pair all single quotes in that string to avoid unexpected formatting results.
All non-quoted left curly brackets must have corresponding right curly brackets in the formatting string. If the string contains imbalanced left curly brackets, it causes a runtime error. For example, "ab {0} de" and "ab }{0} de" are valid formatting strings, but "ab {0'}' de", "ab }{0}{ de" and "''{''{0}" are not.
The placeholders for "int" or "float" values or arrays can include modifiers and formatting tokens to customize how the resulting string represents them.
For example, the placeholder {0,number,#.#) specifies that the result inserts characters representing the arg0 number rounded to one fractional digit.
For detailed information about placeholders and supported formats, refer to the Formatting strings section of our User Manual's Strings page.
The apostrophe (') acts as a quote character rather than a literal character inside formatting strings. If a formatting string has a sequence of characters between two apostrophes, the function's result includes those characters literally. For instance, the substring '{' adds a literal { character to the result instead of treating it as the start of a placeholder. Note that if a formatting string uses apostrophes instead of quotation marks for its enclosing characters, the string must escape any apostrophes within the character sequence using the backslash.
str.format_time()

## [Converts the time timestamp into a string formatted according to format and timezone.](https://www.tradingview.com/pine-script-reference/v6/#fun_Convertsthetimetimestampintoastringformattedaccordingtoformatandtimezone.)

Converts the time timestamp into a string formatted according to format and timezone.
Syntax
str.format_time(time, format, timezone) → series string
Arguments
time (series int) UNIX time, in milliseconds.
format (series string) A format string specifying the date/time representation of the time in the returned string. All letters used in the string, except those escaped by single quotation marks ', are considered formatting tokens and will be used as a formatting instruction. Refer to the Remarks section for a list of the most useful tokens. Optional. The default is "yyyy-MM-dd'T'HH:mm:ssZ", which represents the ISO 8601 standard.
timezone (series string) Allows adjusting the returned value to a time zone specified in either UTC/GMT notation (e.g., "UTC-5", "GMT+0530") or as an IANA time zone database name (e.g., "America/New_York"). Optional. The default is syminfo.timezone.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("str.format_time")
if timeframe.change("1D")
    formattedTime = str.format_time(time, "yyyy-MM-dd HH:mm", syminfo.timezone)
    label.new(bar_index, high, formattedTime)
Returns
The formatted string.
Remarks
The M, d, h, H, m and s tokens can all be doubled to generate leading zeros. For example, the month of January will display as 1 with M, or 01 with MM.
The most frequently used formatting tokens are:
y - Year. Use yy to output the last two digits of the year or yyyy to output all four. Year 2000 will be 00 with yy or 2000 with yyyy.
M - Month. Not to be confused with lowercase m, which stands for minute.
d - Day of the month.
a - AM/PM postfix.
h - Hour in the 12-hour format. The last hour of the day will be 11 in this format.
H - Hour in the 24-hour format. The last hour of the day will be 23 in this format.
m - Minute.
s - Second.
S - Fractions of a second.
Z - Timezone, the HHmm offset from UTC, preceded by either + or -.
str.length()3 overloads

## [Returns an integer corresponding to the amount of chars in that string.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsanintegercorrespondingtotheamountofcharsinthatstring.)

Returns an integer corresponding to the amount of chars in that string.
Syntax & Overloads
str.length(string) → const int
str.length(string) → simple int
str.length(string) → series int
Arguments
string (const string) Source string.
Returns
The number of chars in source string.
str.lower()3 overloads

## [Returns a new string with all letters converted to lowercase.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsanewstringwithalllettersconvertedtolowercase.)

Returns a new string with all letters converted to lowercase.
Syntax & Overloads
str.lower(source) → const string
str.lower(source) → simple string
str.lower(source) → series string
Arguments
source (const string) String to be converted.
Returns
A new string with all letters converted to lowercase.
See also
str.upper()
str.match()2 overloads

## [Returns the new substring of the source string if it matches a regex regular expression, an empty string otherwise.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthenewsubstringofthesourcestringifitmatchesaregexregularexpressionanemptystringotherwise.)

Returns the new substring of the source string if it matches a regex regular expression, an empty string otherwise.
Syntax & Overloads
str.match(source, regex) → simple string
str.match(source, regex) → series string
Arguments
source (simple string) Source string.
regex (simple string) The regular expression to which this string is to be matched.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("str.match")

## [s = input.string("It's time to sell some NASDAQ:AAPL!")](https://www.tradingview.com/pine-script-reference/v6/#fun_sinput.stringItstimetosellsomeNASDAQAAPL)

s = input.string("It's time to sell some NASDAQ:AAPL!")

## [// finding first substring that matches regular expression "[\w]+:[\w]+"](https://www.tradingview.com/pine-script-reference/v6/#fun_findingfirstsubstringthatmatchesregularexpressionww)

// finding first substring that matches regular expression "[\w]+:[\w]+"
var string tickerid = str.match(s, "[\\w]+:[\\w]+")

## [if barstate.islastconfirmedhistory](https://www.tradingview.com/pine-script-reference/v6/#fun_ifbarstate.islastconfirmedhistory)

if barstate.islastconfirmedhistory
    label.new(bar_index, high, text = tickerid) // "NASDAQ:AAPL"
Returns
The new substring of the source string if it matches a regex regular expression, an empty string otherwise.
Remarks
Function returns first occurrence of the regular expression in the source string.
The backslash "\" symbol in theregex string needs to be escaped with additional backslash, e.g. "\\d" stands for regular expression "\d".
See also
str.contains()
str.substring()
str.pos()3 overloads

## [Returns the position of the first occurrence of the str string in the source string, 'na' otherwise.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthepositionofthefirstoccurrenceofthestrstringinthesourcestringnaotherwise.)

Returns the position of the first occurrence of the str string in the source string, 'na' otherwise.
Syntax & Overloads
str.pos(source, str) → const int
str.pos(source, str) → simple int
str.pos(source, str) → series int
Arguments
source (const string) Source string.
str (const string) The substring to search for.
Returns
Position of the str string in the source string.
Remarks
Strings indexing starts at 0.
See also
str.contains()
str.match()
str.substring()
str.repeat()4 overloads

## [Constructs a new string containing the source string repeated repeat times with the separator injected between each repeated instance.](https://www.tradingview.com/pine-script-reference/v6/#fun_Constructsanewstringcontainingthesourcestringrepeatedrepeattimeswiththeseparatorinjectedbetweeneachrepeatedinstance.)

Constructs a new string containing the source string repeated repeat times with the separator injected between each repeated instance.
Syntax & Overloads
str.repeat(source, repeat, separator) → const string
str.repeat(source, repeat, separator) → input string
str.repeat(source, repeat, separator) → simple string
str.repeat(source, repeat, separator) → series string
Arguments
source (const string) String to repeat.
repeat (const int) Number of times to repeat the source string. Must be greater than or equal to 0.
separator (const string) String to inject between repeated values. Optional. The default is empty string.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("str.repeat")
repeat = str.repeat("?", 3, ",") // Returns "?,?,?"
label.new(bar_index,close,repeat)
Remarks
Returns na if the source is na.
str.replace()3 overloads

## [Returns a new string with the Nth occurrence of the target string replaced by the replacement string, where N is specified in occurrence.](https://www.tradingview.com/pine-script-reference/v6/#fun_ReturnsanewstringwiththeNthoccurrenceofthetargetstringreplacedbythereplacementstringwhereNisspecifiedinoccurrence.)

Returns a new string with the Nth occurrence of the target string replaced by the replacement string, where N is specified in occurrence.
Syntax & Overloads
str.replace(source, target, replacement, occurrence) → const string
str.replace(source, target, replacement, occurrence) → simple string
str.replace(source, target, replacement, occurrence) → series string
Arguments
source (const string) Source string.
target (const string) String to be replaced.
replacement (const string) String to be inserted instead of the target string.
occurrence (const int) N-th occurrence of the target string to replace. Indexing starts at 0 for the first match. Optional. Default value is 0.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("str.replace")
var source = "FTX:BTCUSD / FTX:BTCEUR"

## [// Replace first occurrence of "FTX" with "BINANCE" replacement string](https://www.tradingview.com/pine-script-reference/v6/#fun_ReplacefirstoccurrenceofFTXwithBINANCEreplacementstring)

// Replace first occurrence of "FTX" with "BINANCE" replacement string
var newSource = str.replace(source, "FTX", "BINANCE", 0)

## [if barstate.islastconfirmedhistory](https://www.tradingview.com/pine-script-reference/v6/#fun_ifbarstate.islastconfirmedhistory)

if barstate.islastconfirmedhistory
    // Display "BINANCE:BTCUSD / FTX:BTCEUR"
    label.new(bar_index, high, text = newSource)
Returns
Processed string.
See also
str.replace_all()
str.match()
str.replace_all()2 overloads

## [Replaces each occurrence of the target string in the source string with the replacement string.](https://www.tradingview.com/pine-script-reference/v6/#fun_Replaceseachoccurrenceofthetargetstringinthesourcestringwiththereplacementstring.)

Replaces each occurrence of the target string in the source string with the replacement string.
Syntax & Overloads
str.replace_all(source, target, replacement) → simple string
str.replace_all(source, target, replacement) → series string
Arguments
source (simple string) Source string.
target (simple string) String to be replaced.
replacement (simple string) String to be substituted for each occurrence of target string.
Returns
Processed string.
str.split()

## [Divides a string into an array of substrings and returns its array id.](https://www.tradingview.com/pine-script-reference/v6/#fun_Dividesastringintoanarrayofsubstringsandreturnsitsarrayid.)

Divides a string into an array of substrings and returns its array id.
Syntax
str.split(string, separator) → array<string>
Arguments
string (series string) Source string.
separator (series string) The string separating each substring.
Returns
The id of an array of strings.
str.startswith()3 overloads

## [Returns true if the source string starts with the substring specified in str, false otherwise.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnstrueifthesourcestringstartswiththesubstringspecifiedinstrfalseotherwise.)

Returns true if the source string starts with the substring specified in str, false otherwise.
Syntax & Overloads
str.startswith(source, str) → const bool
str.startswith(source, str) → simple bool
str.startswith(source, str) → series bool
Arguments
source (const string) Source string.
str (const string) The substring to search for.
Returns
True if the source string starts with the substring specified in str, false otherwise.
See also
str.endswith()
str.substring()3 overloads

## [Returns a new string that is a substring of the source string. The substring begins with the character at the index specified by begin_pos and extends to 'end_pos - 1' of the source string.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsanewstringthatisasubstringofthesourcestring.Thesubstringbeginswiththecharacterattheindexspecifiedbybegin_posandextendstoend_pos-1ofthesourcestring.)

Returns a new string that is a substring of the source string. The substring begins with the character at the index specified by begin_pos and extends to 'end_pos - 1' of the source string.
Syntax & Overloads
str.substring(source, begin_pos, end_pos) → const string
str.substring(source, begin_pos, end_pos) → simple string
str.substring(source, begin_pos, end_pos) → series string
Arguments
source (const string) Source string from which to extract the substring.
begin_pos (const int) The beginning position of the extracted substring. It is inclusive (the extracted substring includes the character at that position).
end_pos (const int) The ending position. It is exclusive (the extracted string does NOT include that position's character). Optional. The default is the length of the source string.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("str.substring", overlay = true)
sym= input.symbol("NASDAQ:AAPL")
pos = str.pos(sym, ":") // Get position of ":" character
tkr= str.substring(sym, pos+1) // "AAPL"
if barstate.islastconfirmedhistory
    label.new(bar_index, high, text = tkr)
Returns
The substring extracted from the source string.
Remarks
Strings indexing starts from 0. If begin_pos is equal to end_pos, the function returns an empty string.
See also
str.contains()
str.pos()
str.match()
str.tonumber()4 overloads

## [Converts a value represented in string to its "float" equivalent.](https://www.tradingview.com/pine-script-reference/v6/#fun_Convertsavaluerepresentedinstringtoitsfloatequivalent.)

Converts a value represented in string to its "float" equivalent.
Syntax & Overloads
str.tonumber(string) → const float
str.tonumber(string) → input float
str.tonumber(string) → simple float
str.tonumber(string) → series float
Arguments
string (const string) String containing the representation of an integer or floating point value.
Returns
A "float" equivalent of the value in string. If the value is not a properly formed integer or floating point value, the function returns na.
str.tostring()5 overloads

## [Syntax & Overloads](https://www.tradingview.com/pine-script-reference/v6/#fun_SyntaxOverloads)

Syntax & Overloads
str.tostring(value) → const string
str.tostring(value, format) → simple string
str.tostring(value, format) → series string
str.tostring(value) → simple string
str.tostring(value) → series string
Arguments
value (const enum) Value or array ID whose elements are converted to a string.
Returns
The string representation of the value argument.
If the value argument is a string, it is returned as is.
When the value is na, the function returns the string "NaN".
Remarks
The formatting of float values will also round those values when necessary, e.g. str.tostring(3.99, '#') will return "4".
To display trailing zeros, use '0' instead of '#'. For example, '#.000'.
When using format.mintick, the value will be rounded to the nearest number that can be divided by syminfo.mintick without the remainder. The string is returned with trailing zeros.
If the x argument is a string, the same string value will be returned.
Bool type arguments return "true" or "false".
When x is na, the function returns "NaN".
str.trim()4 overloads

## [Constructs a new string with all consecutive whitespaces and other control characters (e.g., “\n”, “\t”, etc.) removed from the left and right of the source.](https://www.tradingview.com/pine-script-reference/v6/#fun_Constructsanewstringwithallconsecutivewhitespacesandothercontrolcharacterse.g.ntetc.removedfromtheleftandrightofthesource.)

Constructs a new string with all consecutive whitespaces and other control characters (e.g., “\n”, “\t”, etc.) removed from the left and right of the source.
Syntax & Overloads
str.trim(source) → const string
str.trim(source) → input string
str.trim(source) → simple string
str.trim(source) → series string
Arguments
source (const string) String to trim.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
indicator("str.trim")
trim = str.trim("    abc    ") // Returns "abc"
label.new(bar_index,close,trim)
Remarks
Returns an empty string ("") if the result is empty after the trim or if the source is na.
str.upper()3 overloads

## [Returns a new string with all letters converted to uppercase.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsanewstringwithalllettersconvertedtouppercase.)

Returns a new string with all letters converted to uppercase.
Syntax & Overloads
str.upper(source) → const string
str.upper(source) → simple string
str.upper(source) → series string
Arguments
source (const string) String to be converted.
Returns
A new string with all letters converted to uppercase.
See also
str.lower()
strategy()

## [This declaration statement designates the script as a strategy and sets a number of strategy-related properties.](https://www.tradingview.com/pine-script-reference/v6/#fun_Thisdeclarationstatementdesignatesthescriptasastrategyandsetsanumberofstrategy-relatedproperties.)

This declaration statement designates the script as a strategy and sets a number of strategy-related properties.
Learn more
Syntax
strategy(title, shorttitle, overlay, format, precision, scale, pyramiding, calc_on_order_fills, calc_on_every_tick, max_bars_back, backtest_fill_limits_assumption, default_qty_type, default_qty_value, initial_capital, currency, slippage, commission_type, commission_value, process_orders_on_close, close_entries_rule, margin_long, margin_short, explicit_plot_zorder, max_lines_count, max_labels_count, max_boxes_count, calc_bars_count, risk_free_rate, use_bar_magnifier, fill_orders_on_standard_ohlc, max_polylines_count, dynamic_requests, behind_chart) → void
Arguments
title (const string) The title of the script. It is displayed on the chart when no shorttitle argument is used, and becomes the publication's default title when publishing the script.
shorttitle (const string) The script's display name on charts. If specified, it will replace the title argument in most chart-related windows. Optional. The default is the argument used for title.
overlay (const bool) If true, the script's visuals appear on the main chart pane if the user adds it to the chart directly, or in another script's pane if the user applies it to that script. If false, the script's visuals appear in a separate pane. Changes to the overlay value apply only after the user adds the script to the chart again. Additionally, if the user moves the script to another pane by selecting a "Move to" option in the script's "More" menu, it does not move back to its original pane after any updates to the source code. The default is false. Strategy-specific labels that display entries and exits will be displayed over the main chart regardless of this setting.
format (const string) Specifies the formatting of the script's displayed values. Possible values: format.inherit, format.price, format.volume, format.percent. Optional. The default is format.inherit.
precision (const int) Specifies the number of digits after the floating point of the script's displayed values. Must be a non-negative integer no greater than 16. If format is set to format.inherit and precision is specified, the format will instead be set to format.price. When the function's format parameter uses format.volume, the precision parameter will not affect the result, as the decimal precision rules defined by format.volume supersede other precision settings. Optional. The default is inherited from the precision of the chart's symbol.
scale (const scale_type) Optional. Determines the location of the script's price scale and the scaling behavior of the script's visuals. Possible values are scale.right, scale.left, and scale.none. If specified and the script overlays on the main chart pane or another script's pane, the script scales its visuals independently to fit the pane's visual space. If the script occupies the same pane as the main chart or another script, scale.right or scale.left adds a separate price scale for the script to the left or right side of that pane. If the script occupies a separate pane, either argument positions the price scale for that pane on the left or right side without adding a new scale. If the argument is scale.none, which is valid only if the overlay argument is true, the script displays plotted numbers directly on the scale of the existing pane, or displays values on a new price scale if the user moves it to a new pane. Changes to the argument apply only after the user adds the script to the chart again. If not specified, the script uses the main price scale for the pane it occupies, and it does not scale its visuals separately if it overlays on an existing pane.
pyramiding (const int) The maximum number of entries allowed in the same direction. If the value is 0, only one entry order in the same direction can be opened, and additional entry orders are rejected. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is 0.
calc_on_order_fills (const bool) Specifies whether the strategy should be recalculated after an order is filled. If true, the strategy recalculates after an order is filled, as opposed to recalculating only when the bar closes. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is false.
calc_on_every_tick (const bool) Specifies whether the strategy should be recalculated on each realtime tick. If true, when the strategy is running on a realtime bar, it will recalculate on each chart update. If false, the strategy only calculates when the realtime bar closes. The argument used does not affect strategy calculation on historical data. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is false.
max_bars_back (const int) The length of the historical buffer the script keeps for every variable and function, which determines how many past values can be referenced using the [] history-referencing operator. The required buffer size is automatically detected by the Pine Script® runtime. Using this parameter is only necessary when a runtime error occurs because automatic detection fails. More information on the underlying mechanics of the historical buffer can be found in our Help Center. Optional. The default is 0.
backtest_fill_limits_assumption (const int) Limit order execution threshold in ticks. When it is used, limit orders are only filled if the market price exceeds the order's limit level by the specified number of ticks. Optional. The default is 0.
default_qty_type (const string) Specifies the units used for default_qty_value. Possible values are: strategy.fixed for contracts/shares/lots, strategy.cash for currency amounts, or strategy.percent_of_equity for a percentage of available equity. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is strategy.fixed.
default_qty_value (const int/float) The default quantity to trade, in units determined by the argument used with the default_qty_type parameter. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is 1.
initial_capital (const int/float) The amount of funds initially available for the strategy to trade, in units of currency. Optional. The default is 1000000.
currency (const string) Currency used by the strategy in currency-related calculations. Market positions are still opened by converting currency into the chart symbol's currency. The conversion rate depends on the previous daily value of a corresponding currency pair from the most popular exchange. A spread symbol is used if no exchange provides the rate directly. Possible values: a "string" representing a valid currency code (e.g., "USD" or "USDT") or a constant from the currency.*namespace (e.g., currency.USD or currency.USDT). The default is syminfo.currency.
slippage (const int) Slippage expressed in ticks. This value is added to or subtracted from the fill price of market/stop orders to make the fill price less favorable for the strategy. E.g., if syminfo.mintick is 0.01 and slippage is set to 5, a long market order will enter at 5* 0.01 = 0.05 points above the actual price. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is 0.
commission_type (const string) Determines what the number passed to the commission_value expresses: strategy.commission.percent for a percentage of the cash volume of the order, strategy.commission.cash_per_contract for currency per contract, strategy.commission.cash_per_order for currency per order. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is strategy.commission.percent.
commission_value (const int/float) Commission applied to the strategy's orders in units determined by the argument passed to the commission_type parameter. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is 0.
process_orders_on_close (const bool) When set to true, generates an additional attempt to execute orders after a bar closes and strategy calculations are completed. If the orders are market orders, the broker emulator executes them before the next bar's open. If the orders are price-dependent, they will only be filled if the price conditions are met. This option is useful if you wish to close positions on the current bar. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. The default is false.
close_entries_rule (const string) Determines the order in which trades are closed. Possible values are: "FIFO" (First-In, First-Out) if the earliest exit order must close the earliest entry order, or "ANY" if the orders are closed based on the from_entry parameter of the strategy.exit() function. "FIFO" can only be used with stocks, futures and US forex (NFA Compliance Rule 2-43b), while "ANY" is allowed in non-US forex. Optional. The default is "FIFO".
margin_long (const int/float) Margin long is the percentage of the purchase price of a security that must be covered by cash or collateral for long positions. Must be a non-negative number. The logic used to simulate margin calls is explained in the Help Center. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. If the value is 0, the strategy does not enforce any limits on position size. The default is 100, in which case the strategy only uses its own funds and the long positions cannot be margin called.
margin_short (const int/float) Margin short is the percentage of the purchase price of a security that must be covered by cash or collateral for short positions. Must be a non-negative number. The logic used to simulate margin calls is explained in the Help Center. This setting can also be changed in the strategy's "Settings/Properties" tab. Optional. If the value is 0, the strategy does not enforce any limits on position size. The default is 100, in which case the strategy only uses its own funds. Note that even with no margin used, short positions can be margin called if the loss exceeds available funds.
explicit_plot_zorder (const bool) Specifies the order in which the script's plots, fills, and hlines are rendered. If true, plots are drawn in the order in which they appear in the script's code, each newer plot being drawn above the previous ones. This only applies to plot*() functions, fill(), and hline(). Optional. The default is false.
max_lines_count (const int) The number of last line drawings displayed. Possible values: 1-500. Optional. The default is 50.
max_labels_count (const int) The number of last label drawings displayed. Possible values: 1-500. Optional. The default is 50.
max_boxes_count (const int) The number of last box drawings displayed. Possible values: 1-500. Optional. The default is 50.
calc_bars_count (const int) Limits the initial calculation of a script to the last number of bars specified. When specified, a "Calculated bars" field will be included in the "Calculation" section of the script's "Settings/Inputs" tab. Optional. The default is 0, in which case the script executes on all available bars.
risk_free_rate (const int/float) The risk-free rate of return is the annual percentage change in the value of an investment with minimal or zero risk. It is used to calculate the Sharpe and Sortino ratios. Optional. The default is 2.
use_bar_magnifier (const bool) Optional. When true, the Broker Emulator uses lower timeframe data during backtesting on historical bars to achieve more realistic results. The default is false. Only Premium and higher-tier plans have access to this feature.
fill_orders_on_standard_ohlc (const bool) When true, forces strategies running on Heikin Ashi charts to fill orders using actual OHLC prices, for more realistic results. Optional. The default is false.
max_polylines_count (const int) The number of last polyline drawings displayed. Possible values: 1-100. The count is approximate; more drawings than the specified count may be displayed. Optional. The default is 50.
dynamic_requests (const bool) Specifies whether the script can dynamically call functions from the request.*() namespace. Dynamic request.*() calls are allowed within the local scopes of conditional structures (e.g., if), loops (e.g., for), and exported functions. Additionally, such calls allow "series" arguments for many of their parameters. Optional. The default is true. See the User Manual's Dynamic requests section for more information.
behind_chart (const bool) Optional. Controls whether all plots and drawings appear behind the chart display (if true) or in front of it (if false). This parameter only takes effect when the overlay parameter is true. The default is true.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("My strategy", overlay = true)

## [// Enter long by market if current open is greater than previous high.](https://www.tradingview.com/pine-script-reference/v6/#fun_Enterlongbymarketifcurrentopenisgreaterthanprevioushigh.)

// Enter long by market if current open is greater than previous high.
if open > high[1]
    strategy.entry("Long", strategy.long, 1)
// Generate a full exit bracket (profit 10 points, loss 5 points per contract) from the entry named "Long".
strategy.exit("Exit", "Long", profit = 10, loss = 5)
Remarks
You can learn more about strategies in our User Manual.
Every strategy script must have one strategy() call.
Strategies using calc_on_every_tick = true parameter may calculate differently on historical and realtime bars, which causes repainting.
Strategies always use the chart's prices to enter and exit positions. Using them on non-standard chart types (Heikin Ashi, Renko, etc.) will produce misleading results, as their prices are synthetic. Backtesting on non-standard charts is thus not recommended.
The maximum number of orders a strategy can open, unless it uses Deep Backtesting mode, is 9000. If the strategy exceeds this limit, it removes the oldest order's information when a new entry appears in the "List of Trades" tab. The strategy.closedtrades.*() functions return na for trades opened or closed by removed orders. To retrieve the index of the oldest available closed trade, use the strategy.closedtrades.first_index variable.
See also
indicator()
library()
strategy.cancel()

## [Cancels a pending or unfilled order with a specific identifier. If multiple unfilled orders share the same ID, calling this command with that ID as the id argument cancels all of them. If a script calls this command with an id representing the ID of a filled order, it has no effect.](https://www.tradingview.com/pine-script-reference/v6/#fun_Cancelsapendingorunfilledorderwithaspecificidentifier.IfmultipleunfilledorderssharethesameIDcallingthiscommandwiththatIDastheidargumentcancelsallofthem.IfascriptcallsthiscommandwithanidrepresentingtheIDofafilledorderithasnoeffect.)

Cancels a pending or unfilled order with a specific identifier. If multiple unfilled orders share the same ID, calling this command with that ID as the id argument cancels all of them. If a script calls this command with an id representing the ID of a filled order, it has no effect.
This command is most useful when working with price-based orders (e.g., limit orders). Calls to this command can also cancel market orders, but only if they execute on the same ticks as the order placement commands.
Learn more
Syntax
strategy.cancel(id) → void
Arguments
id (series string) The identifier of the unfilled order to cancel.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy(title = "Order cancellation demo")

## [conditionForBuy = open > high[1]](https://www.tradingview.com/pine-script-reference/v6/#fun_conditionForBuyopenhigh1)

conditionForBuy = open > high[1]
if conditionForBuy
    strategy.entry("Long", strategy.long, 1, limit = low) // Enter long using limit order at low price of current bar if `conditionForBuy` is `true`.
if not conditionForBuy
    strategy.cancel("Long") // Cancel the entry order with name "Long" if `conditionForBuy` is `false`.
strategy.cancel_all()

## [Cancels all pending or unfilled orders, regardless of their identifiers.](https://www.tradingview.com/pine-script-reference/v6/#fun_Cancelsallpendingorunfilledordersregardlessoftheiridentifiers.)

Cancels all pending or unfilled orders, regardless of their identifiers.
This command is most useful when working with price-based orders (e.g., limit orders). Calls to this command can also cancel market orders, but only if they execute on the same ticks as the order placement commands.
Learn more
Syntax
strategy.cancel_all() → void
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy(title = "Cancel all orders demo")
conditionForBuy1 = open > high[1]
if conditionForBuy1
    strategy.entry("Long entry 1", strategy.long, 1, limit = low) // Enter long using a limit order if `conditionForBuy1` is `true`.
conditionForBuy2 = conditionForBuy1 and open[1] > high[2]
float lowest2 = ta.lowest(low, 2)
if conditionForBuy2
    strategy.entry("Long entry 2", strategy.long, 1, limit = lowest2) // Enter long using a limit order if `conditionForBuy2` is `true`.
conditionForStopTrading = open < lowest2
if conditionForStopTrading
    strategy.cancel_all() // Cancel both limit orders if `conditionForStopTrading` is `true`.
strategy.close()

## [Creates an order to exit from the part of a position opened by entry orders with a specific identifier. If multiple entries in the position share the same ID, the orders from this command apply to all those entries, starting from the first open trade, when its calls use that ID as the id argument.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createsanordertoexitfromthepartofapositionopenedbyentryorderswithaspecificidentifier.IfmultipleentriesinthepositionsharethesameIDtheordersfromthiscommandapplytoallthoseentriesstartingfromthefirstopentradewhenitscallsusethatIDastheidargument.)

Creates an order to exit from the part of a position opened by entry orders with a specific identifier. If multiple entries in the position share the same ID, the orders from this command apply to all those entries, starting from the first open trade, when its calls use that ID as the id argument.
This command always generates market orders. To exit from a position using price-based orders (e.g., stop-loss orders), use the strategy.exit() command.
Learn more
Syntax
strategy.close(id, comment, qty, qty_percent, alert_message, immediately, disable_alert) → void
Arguments
id (series string) The entry identifier of the open trades to close.
comment (series string) Optional. Additional notes on the filled order. If the value is not an empty string, the Strategy Tester and the chart show this text for the order instead of the automatically generated exit identifier. The default is an empty string.
qty (series int/float) Optional. The number of contracts/lots/shares/units to close when an exit order fills. If specified, the command uses this value instead of qty_percent to determine the order size. The default is na, which means the order size depends on the qty_percent value.
qty_percent (series int/float) Optional. A value between 0 and 100 representing the percentage of the open trade quantity to close when an exit order fills. The percentage calculation depends on the total size of the open trades with the id entry identifier. The command ignores this parameter if the qty value is not na. The default is 100.
alert_message (series string) Optional. Custom text for the alert that fires when an order fills. If the "Message" field of the "Create Alert" dialog box contains the {{strategy.order.alert_message}} placeholder, the alert message replaces the placeholder with this text. The default is an empty string.
immediately (series bool) Optional. If true, the closing order executes on the same tick when the strategy places it, ignoring the strategy properties that restrict execution to the opening tick of the following bar. The default is false.
disable_alert (series bool) Optional. If true when the command creates an order, the strategy does not trigger an alert when that order fills. This parameter accepts a "series" value, meaning users can control which orders trigger alerts when they execute. The default is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("Partial close strategy")

## [// Calculate a 14-bar and 28-bar moving average of `close` prices.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculatea14-barand28-barmovingaverageofcloseprices.)

// Calculate a 14-bar and 28-bar moving average of `close` prices.
float sma14 = ta.sma(close, 14)
float sma28 = ta.sma(close, 28)

## [// Place a market order to enter a long position when `sma14` crosses over `sma28`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Placeamarketordertoenteralongpositionwhensma14crossesoversma28.)

// Place a market order to enter a long position when `sma14` crosses over `sma28`.
if ta.crossover(sma14, sma28)
    strategy.entry("My Long Entry ID", strategy.long)

## [// Place a market order to close the long trade when `sma14` crosses under `sma28`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Placeamarketordertoclosethelongtradewhensma14crossesundersma28.)

// Place a market order to close the long trade when `sma14` crosses under `sma28`.
if ta.crossunder(sma14, sma28)
    strategy.close("My Long Entry ID", "50% market close", qty_percent = 50)

## [// Plot the position size.](https://www.tradingview.com/pine-script-reference/v6/#fun_Plotthepositionsize.)

// Plot the position size.
plot(strategy.position_size)
Remarks
When a position consists of several open trades and the close_entries_rule in the strategy() declaration statement is "FIFO" (default), a strategy.close() call exits from the position starting with the first open trade. This behavior applies even if the id value is the entry ID of different open trades. However, in that case, the maximum exit order size still depends on the trades opened by orders with the id identifier. For more information, see this section of our User Manual.
strategy.close_all()

## [Creates an order to close an open position completely, regardless of the identifiers of the entry orders that opened or added to it.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createsanordertocloseanopenpositioncompletelyregardlessoftheidentifiersoftheentryordersthatopenedoraddedtoit.)

Creates an order to close an open position completely, regardless of the identifiers of the entry orders that opened or added to it.
This command always generates market orders. To exit from a position using price-based orders (e.g., stop-loss orders), use the strategy.exit() command.
Learn more
Syntax
strategy.close_all(comment, alert_message, immediately, disable_alert) → void
Arguments
comment (series string) Optional. Additional notes on the filled order. If the value is not an empty string, the Strategy Tester and the chart show this text for the order instead of the automatically generated exit identifier. The default is an empty string.
alert_message (series string) Optional. Custom text for the alert that fires when an order fills. If the "Message" field of the "Create Alert" dialog box contains the {{strategy.order.alert_message}} placeholder, the alert message replaces the placeholder with this text. The default is an empty string.
immediately (series bool) Optional. If true, the closing order executes on the same tick when the strategy places it, ignoring the strategy properties that restrict execution to the opening tick of the following bar. The default is false.
disable_alert (series bool) Optional. If true when the command creates an order, the strategy does not trigger an alert when that order fills. This parameter accepts a "series" value, meaning users can control which orders trigger alerts when they execute. The default is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("Multi-entry close strategy")

## [// Calculate a 14-bar and 28-bar moving average of `close` prices.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculatea14-barand28-barmovingaverageofcloseprices.)

// Calculate a 14-bar and 28-bar moving average of `close` prices.
float sma14 = ta.sma(close, 14)
float sma28 = ta.sma(close, 28)

## [// Place a market order to enter a long trade every time `sma14` crosses over `sma28`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Placeamarketordertoenteralongtradeeverytimesma14crossesoversma28.)

// Place a market order to enter a long trade every time `sma14` crosses over `sma28`.
if ta.crossover(sma14, sma28)
    strategy.order("My Long Entry ID " + str.tostring(strategy.opentrades), strategy.long)

## [// Place a market order to close the entire position every 500 bars.](https://www.tradingview.com/pine-script-reference/v6/#fun_Placeamarketordertoclosetheentirepositionevery500bars.)

// Place a market order to close the entire position every 500 bars.
if bar_index % 500 == 0
    strategy.close_all()

## [// Plot the position size.](https://www.tradingview.com/pine-script-reference/v6/#fun_Plotthepositionsize.)

// Plot the position size.
plot(strategy.position_size)
strategy.closedtrades.commission()

## [Returns the sum of entry and exit fees paid in the closed trade, expressed in strategy.account_currency.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthesumofentryandexitfeespaidintheclosedtradeexpressedinstrategy.account_currency.)

Returns the sum of entry and exit fees paid in the closed trade, expressed in strategy.account_currency.
Syntax
strategy.closedtrades.commission(trade_num) → series float
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("`strategy.closedtrades.commission` Example", commission_type = strategy.commission.percent, commission_value = 0.1)

## [// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.](https://www.tradingview.com/pine-script-reference/v6/#fun_Strategycallstoenterlongtradesevery15barsandexitlongtradesevery20bars.)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

## [// Plot total fees for the latest closed trade.](https://www.tradingview.com/pine-script-reference/v6/#fun_Plottotalfeesforthelatestclosedtrade.)

// Plot total fees for the latest closed trade.
plot(strategy.closedtrades.commission(strategy.closedtrades - 1))
See also
strategy()
strategy.opentrades.commission()
strategy.closedtrades.entry_bar_index()

## [Returns the bar_index of the closed trade's entry.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthebar_indexoftheclosedtradesentry.)

Returns the bar_index of the closed trade's entry.
Syntax
strategy.closedtrades.entry_bar_index(trade_num) → series int
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("strategy.closedtrades.entry_bar_index Example")
// Enter long trades on three rising bars; exit on two falling bars.
if ta.rising(close, 3)
    strategy.entry("Long", strategy.long)
if ta.falling(close, 2)
    strategy.close("Long")
// Function that calculates the average amount of bars in a trade.
avgBarsPerTrade() =>
    sumBarsPerTrade = 0
    for tradeNo = 0 to strategy.closedtrades - 1
        // Loop through all closed trades, starting with the oldest.
        sumBarsPerTrade += strategy.closedtrades.exit_bar_index(tradeNo) - strategy.closedtrades.entry_bar_index(tradeNo) + 1
    result = nz(sumBarsPerTrade / strategy.closedtrades)
plot(avgBarsPerTrade())
See also
strategy.closedtrades.exit_bar_index()
strategy.opentrades.entry_bar_index()
strategy.closedtrades.entry_comment()

## [Returns the comment message of the closed trade's entry, or na if there is no entry with this trade_num.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthecommentmessageoftheclosedtradesentryornaifthereisnoentrywiththistrade_num.)

Returns the comment message of the closed trade's entry, or na if there is no entry with this trade_num.
Syntax
strategy.closedtrades.entry_comment(trade_num) → series string
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("`strategy.closedtrades.entry_comment()` Example", overlay = true)

## [stopPrice = open * 1.01](https://www.tradingview.com/pine-script-reference/v6/#fun_stopPriceopen1.01)

stopPrice = open * 1.01

## [longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))](https://www.tradingview.com/pine-script-reference/v6/#fun_longConditionta.crossoverta.smaclose14ta.smaclose28)

longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))

## [if (longCondition)](https://www.tradingview.com/pine-script-reference/v6/#fun_iflongCondition)

if (longCondition)
    strategy.entry("Long", strategy.long, stop = stopPrice, comment = str.tostring(stopPrice, "#.####"))
    strategy.exit("EXIT", trail_points = 1000, trail_offset = 0)

## [var testTable = table.new(position.top_right, 1, 3, color.orange, border_width = 1)](https://www.tradingview.com/pine-script-reference/v6/#fun_vartestTabletable.newposition.top_right13color.orangeborder_width1)

var testTable = table.new(position.top_right, 1, 3, color.orange, border_width = 1)

## [if barstate.islastconfirmedhistory or barstate.isrealtime](https://www.tradingview.com/pine-script-reference/v6/#fun_ifbarstate.islastconfirmedhistoryorbarstate.isrealtime)

if barstate.islastconfirmedhistory or barstate.isrealtime
    table.cell(testTable, 0, 0, 'Last closed trade:')
    table.cell(testTable, 0, 1, "Order stop price value: " + strategy.closedtrades.entry_comment(strategy.closedtrades - 1))
    table.cell(testTable, 0, 2, "Actual Entry Price: " + str.tostring(strategy.closedtrades.entry_price(strategy.closedtrades - 1)))
See also
strategy()
strategy.entry()
strategy.closedtrades
strategy.closedtrades.entry_id()

## [Returns the id of the closed trade's entry.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnstheidoftheclosedtradesentry.)

Returns the id of the closed trade's entry.
Syntax
strategy.closedtrades.entry_id(trade_num) → series string
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("strategy.closedtrades.entry_id Example", overlay = true)

## [// Enter a short position and close at the previous to last bar.](https://www.tradingview.com/pine-script-reference/v6/#fun_Enterashortpositionandcloseattheprevioustolastbar.)

// Enter a short position and close at the previous to last bar.
if bar_index == 1
    strategy.entry("Short at bar #" + str.tostring(bar_index), strategy.short)
if bar_index == last_bar_index - 2
    strategy.close_all()

## [// Display ID of the last entry position.](https://www.tradingview.com/pine-script-reference/v6/#fun_DisplayIDofthelastentryposition.)

// Display ID of the last entry position.
if barstate.islastconfirmedhistory
    label.new(last_bar_index, high, "Last Entry ID is: " + strategy.closedtrades.entry_id(strategy.closedtrades - 1))
Returns
Returns the id of the closed trade's entry.
Remarks
The function returns na if trade_num is not in the range: 0 to strategy.closedtrades-1.
See also
strategy.closedtrades.entry_bar_index()
strategy.closedtrades.entry_price()
strategy.closedtrades.entry_time()
strategy.closedtrades.entry_price()

## [Returns the price of the closed trade's entry.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthepriceoftheclosedtradesentry.)

Returns the price of the closed trade's entry.
Syntax
strategy.closedtrades.entry_price(trade_num) → series float
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("strategy.closedtrades.entry_price Example 1")

## [// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.](https://www.tradingview.com/pine-script-reference/v6/#fun_Strategycallstoenterlongtradesevery15barsandexitlongtradesevery20bars.)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

## [// Return the entry price for the latest entry.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returntheentrypriceforthelatestentry.)

// Return the entry price for the latest entry.
entryPrice = strategy.closedtrades.entry_price(strategy.closedtrades - 1)

## [plot(entryPrice, "Long entry price")](https://www.tradingview.com/pine-script-reference/v6/#fun_plotentryPriceLongentryprice)

plot(entryPrice, "Long entry price")
Example

## [// Calculates the average profit percentage for all closed trades.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculatestheaverageprofitpercentageforallclosedtrades.)

// Calculates the average profit percentage for all closed trades.
//@version=6
strategy("strategy.closedtrades.entry_price Example 2")

## [// Strategy calls to create single short and long trades](https://www.tradingview.com/pine-script-reference/v6/#fun_Strategycallstocreatesingleshortandlongtrades)

// Strategy calls to create single short and long trades
if bar_index == last_bar_index - 15
    strategy.entry("Long Entry", strategy.long)
else if bar_index == last_bar_index - 10
    strategy.close("Long Entry")
    strategy.entry("Short", strategy.short)
else if bar_index == last_bar_index - 5
    strategy.close("Short")

## [// Calculate profit for both closed trades.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculateprofitforbothclosedtrades.)

// Calculate profit for both closed trades.
profitPct = 0.0
for tradeNo = 0 to strategy.closedtrades - 1
    entryP = strategy.closedtrades.entry_price(tradeNo)
    exitP = strategy.closedtrades.exit_price(tradeNo)
    profitPct += (exitP - entryP) / entryP *strategy.closedtrades.size(tradeNo)* 100

## [// Calculate average profit percent for both closed trades.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculateaverageprofitpercentforbothclosedtrades.)

// Calculate average profit percent for both closed trades.
avgProfitPct = nz(profitPct / strategy.closedtrades)

## [plot(avgProfitPct)](https://www.tradingview.com/pine-script-reference/v6/#fun_plotavgProfitPct)

plot(avgProfitPct)
See also
strategy.closedtrades.entry_price()
strategy.closedtrades.exit_price()
strategy.closedtrades.size()
strategy.closedtrades
strategy.closedtrades.entry_time()

## [Returns the UNIX time of the closed trade's entry, expressed in milliseconds..](https://www.tradingview.com/pine-script-reference/v6/#fun_ReturnstheUNIXtimeoftheclosedtradesentryexpressedinmilliseconds..)

Returns the UNIX time of the closed trade's entry, expressed in milliseconds..
Syntax
strategy.closedtrades.entry_time(trade_num) → series int
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("strategy.closedtrades.entry_time Example", overlay = true)

## [// Enter long trades on three rising bars; exit on two falling bars.](https://www.tradingview.com/pine-script-reference/v6/#fun_Enterlongtradesonthreerisingbarsexitontwofallingbars.)

// Enter long trades on three rising bars; exit on two falling bars.
if ta.rising(close, 3)
    strategy.entry("Long", strategy.long)
if ta.falling(close, 2)
    strategy.close("Long")

## [// Calculate the average trade duration](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculatetheaveragetradeduration)

// Calculate the average trade duration
avgTradeDuration() =>
    sumTradeDuration = 0
    for i = 0 to strategy.closedtrades - 1
        sumTradeDuration += strategy.closedtrades.exit_time(i) - strategy.closedtrades.entry_time(i)
    result = nz(sumTradeDuration / strategy.closedtrades)

## [// Display average duration converted to seconds and formatted using 2 decimal points](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayaveragedurationconvertedtosecondsandformattedusing2decimalpoints)

// Display average duration converted to seconds and formatted using 2 decimal points
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(avgTradeDuration() / 1000, "#.##") + " seconds")
See also
strategy.opentrades.entry_time()
strategy.closedtrades.exit_time()
time
strategy.closedtrades.exit_bar_index()

## [Returns the bar_index of the closed trade's exit.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthebar_indexoftheclosedtradesexit.)

Returns the bar_index of the closed trade's exit.
Syntax
strategy.closedtrades.exit_bar_index(trade_num) → series int
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("strategy.closedtrades.exit_bar_index Example 1")

## [// Strategy calls to place a single short trade. We enter the trade at the first bar and exit the trade at 10 bars before the last chart bar.](https://www.tradingview.com/pine-script-reference/v6/#fun_Strategycallstoplaceasingleshorttrade.Weenterthetradeatthefirstbarandexitthetradeat10barsbeforethelastchartbar.)

// Strategy calls to place a single short trade. We enter the trade at the first bar and exit the trade at 10 bars before the last chart bar.
if bar_index == 0
    strategy.entry("Short", strategy.short)
if bar_index == last_bar_index - 10
    strategy.close("Short")

## [// Calculate the amount of bars since the last closed trade.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculatetheamountofbarssincethelastclosedtrade.)

// Calculate the amount of bars since the last closed trade.
barsSinceClosed = strategy.closedtrades > 0 ? bar_index - strategy.closedtrades.exit_bar_index(strategy.closedtrades - 1) : na

## [plot(barsSinceClosed, "Bars since last closed trade")](https://www.tradingview.com/pine-script-reference/v6/#fun_plotbarsSinceClosedBarssincelastclosedtrade)

plot(barsSinceClosed, "Bars since last closed trade")
Example

## [// Calculates the average amount of bars per trade.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculatestheaverageamountofbarspertrade.)

// Calculates the average amount of bars per trade.
//@version=6
strategy("strategy.closedtrades.exit_bar_index Example 2")

## [// Enter long trades on three rising bars; exit on two falling bars.](https://www.tradingview.com/pine-script-reference/v6/#fun_Enterlongtradesonthreerisingbarsexitontwofallingbars.)

// Enter long trades on three rising bars; exit on two falling bars.
if ta.rising(close, 3)
    strategy.entry("Long", strategy.long)
if ta.falling(close, 2)
    strategy.close("Long")

## [// Function that calculates the average amount of bars per trade.](https://www.tradingview.com/pine-script-reference/v6/#fun_Functionthatcalculatestheaverageamountofbarspertrade.)

// Function that calculates the average amount of bars per trade.
avgBarsPerTrade() =>
    sumBarsPerTrade = 0
    for tradeNo = 0 to strategy.closedtrades - 1
        // Loop through all closed trades, starting with the oldest.
        sumBarsPerTrade += strategy.closedtrades.exit_bar_index(tradeNo) - strategy.closedtrades.entry_bar_index(tradeNo) + 1
    result = nz(sumBarsPerTrade / strategy.closedtrades)

## [plot(avgBarsPerTrade())](https://www.tradingview.com/pine-script-reference/v6/#fun_plotavgBarsPerTrade)

plot(avgBarsPerTrade())
See also
bar_index
last_bar_index
strategy.closedtrades.exit_comment()

## [Returns the comment message of the closed trade's exit, or na if there is no entry with this trade_num.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthecommentmessageoftheclosedtradesexitornaifthereisnoentrywiththistrade_num.)

Returns the comment message of the closed trade's exit, or na if there is no entry with this trade_num.
Syntax
strategy.closedtrades.exit_comment(trade_num) → series string
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("`strategy.closedtrades.exit_comment()` Example", overlay = true)

## [longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))](https://www.tradingview.com/pine-script-reference/v6/#fun_longConditionta.crossoverta.smaclose14ta.smaclose28)

longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))
if (longCondition)
    strategy.entry("Long", strategy.long)
    strategy.exit("Exit", stop = open *0.95, limit = close* 1.05, trail_points = 100, trail_offset = 0, comment_profit = "TP", comment_loss = "SL", comment_trailing = "TRAIL")

## [exitStats() =>](https://www.tradingview.com/pine-script-reference/v6/#fun_exitStats)

exitStats() =>
    int slCount = 0
    int tpCount = 0
    int trailCount = 0

## [if strategy.closedtrades > 0](https://www.tradingview.com/pine-script-reference/v6/#fun_ifstrategy.closedtrades0)

if strategy.closedtrades > 0
        for i = 0 to strategy.closedtrades - 1
            switch strategy.closedtrades.exit_comment(i)
                "TP"    => tpCount    += 1
                "SL"    => slCount    += 1
                "TRAIL" => trailCount += 1
    [slCount, tpCount, trailCount]

## [var testTable = table.new(position.top_right, 1, 4, color.orange, border_width = 1)](https://www.tradingview.com/pine-script-reference/v6/#fun_vartestTabletable.newposition.top_right14color.orangeborder_width1)

var testTable = table.new(position.top_right, 1, 4, color.orange, border_width = 1)

## [if barstate.islastconfirmedhistory](https://www.tradingview.com/pine-script-reference/v6/#fun_ifbarstate.islastconfirmedhistory)

if barstate.islastconfirmedhistory
    [slCount, tpCount, trailCount] = exitStats()
    table.cell(testTable, 0, 0, "Closed trades (" + str.tostring(strategy.closedtrades) +") stats:")
    table.cell(testTable, 0, 1, "Stop Loss: " + str.tostring(slCount))
    table.cell(testTable, 0, 2, "Take Profit: " + str.tostring(tpCount))
    table.cell(testTable, 0, 3, "Trailing Stop: " + str.tostring(trailCount))
See also
strategy()
strategy.exit()
strategy.close()
strategy.closedtrades()
strategy.closedtrades.exit_id()

## [Returns the id of the closed trade's exit.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnstheidoftheclosedtradesexit.)

Returns the id of the closed trade's exit.
Syntax
strategy.closedtrades.exit_id(trade_num) → series string
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("strategy.closedtrades.exit_id Example", overlay = true)

## [// Strategy calls to create single short and long trades](https://www.tradingview.com/pine-script-reference/v6/#fun_Strategycallstocreatesingleshortandlongtrades)

// Strategy calls to create single short and long trades
if bar_index == last_bar_index - 15
    strategy.entry("Long Entry", strategy.long)
else if bar_index == last_bar_index - 10
    strategy.entry("Short Entry", strategy.short)

## [// When a new open trade is detected then we create the exit strategy corresponding with the matching entry id](https://www.tradingview.com/pine-script-reference/v6/#fun_Whenanewopentradeisdetectedthenwecreatetheexitstrategycorrespondingwiththematchingentryid)

// When a new open trade is detected then we create the exit strategy corresponding with the matching entry id
// We detect the correct entry id by determining if a position is long or short based on the position quantity
if ta.change(strategy.opentrades) != 0
    posSign = strategy.opentrades.size(strategy.opentrades - 1)
    strategy.exit(posSign > 0 ? "SL Long Exit" : "SL Short Exit", strategy.opentrades.entry_id(strategy.opentrades - 1), stop = posSign > 0 ? high - ta.tr : low + ta.tr)

## [// When a new closed trade is detected then we place a label above the bar with the exit info](https://www.tradingview.com/pine-script-reference/v6/#fun_Whenanewclosedtradeisdetectedthenweplacealabelabovethebarwiththeexitinfo)

// When a new closed trade is detected then we place a label above the bar with the exit info
if ta.change(strategy.closedtrades) != 0
    msg = "Trade closed by: " + strategy.closedtrades.exit_id(strategy.closedtrades - 1)
    label.new(bar_index, high + (3 * ta.tr), msg)
Returns
Returns the id of the closed trade's exit.
Remarks
The function returns na if trade_num is not in the range: 0 to strategy.closedtrades-1.
See also
strategy.closedtrades.exit_bar_index()
strategy.closedtrades.exit_price()
strategy.closedtrades.exit_time()
strategy.closedtrades.exit_price()

## [Returns the price of the closed trade's exit.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthepriceoftheclosedtradesexit.)

Returns the price of the closed trade's exit.
Syntax
strategy.closedtrades.exit_price(trade_num) → series float
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("strategy.closedtrades.exit_price Example 1")

## [// We are creating a long trade every 5 bars](https://www.tradingview.com/pine-script-reference/v6/#fun_Wearecreatingalongtradeevery5bars)

// We are creating a long trade every 5 bars
if bar_index % 5 == 0
    strategy.entry("Long", strategy.long)
strategy.close("Long")

## [// Return the exit price from the latest closed trade.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returntheexitpricefromthelatestclosedtrade.)

// Return the exit price from the latest closed trade.
exitPrice = strategy.closedtrades.exit_price(strategy.closedtrades - 1)

## [plot(exitPrice, "Long exit price")](https://www.tradingview.com/pine-script-reference/v6/#fun_plotexitPriceLongexitprice)

plot(exitPrice, "Long exit price")
Example

## [// Calculates the average profit percentage for all closed trades.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculatestheaverageprofitpercentageforallclosedtrades.)

// Calculates the average profit percentage for all closed trades.
//@version=6
strategy("strategy.closedtrades.exit_price Example 2")

## [// Strategy calls to create single short and long trades.](https://www.tradingview.com/pine-script-reference/v6/#fun_Strategycallstocreatesingleshortandlongtrades.)

// Strategy calls to create single short and long trades.
if bar_index == last_bar_index - 15
    strategy.entry("Long Entry", strategy.long)
else if bar_index == last_bar_index - 10
    strategy.close("Long Entry")
    strategy.entry("Short", strategy.short)
else if bar_index == last_bar_index - 5
    strategy.close("Short")

## [// Calculate profit for both closed trades.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculateprofitforbothclosedtrades.)

// Calculate profit for both closed trades.
profitPct = 0.0
for tradeNo = 0 to strategy.closedtrades - 1
    entryP = strategy.closedtrades.entry_price(tradeNo)
    exitP = strategy.closedtrades.exit_price(tradeNo)
    profitPct += (exitP - entryP) / entryP *strategy.closedtrades.size(tradeNo)* 100

## [// Calculate average profit percent for both closed trades.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculateaverageprofitpercentforbothclosedtrades.)

// Calculate average profit percent for both closed trades.
avgProfitPct = nz(profitPct / strategy.closedtrades)

## [plot(avgProfitPct)](https://www.tradingview.com/pine-script-reference/v6/#fun_plotavgProfitPct)

plot(avgProfitPct)
See also
strategy.closedtrades.entry_price()
strategy.closedtrades.exit_time()

## [Returns the UNIX time of the closed trade's exit, expressed in milliseconds.](https://www.tradingview.com/pine-script-reference/v6/#fun_ReturnstheUNIXtimeoftheclosedtradesexitexpressedinmilliseconds.)

Returns the UNIX time of the closed trade's exit, expressed in milliseconds.
Syntax
strategy.closedtrades.exit_time(trade_num) → series int
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("strategy.closedtrades.exit_time Example 1")

## [// Enter long trades on three rising bars; exit on two falling bars.](https://www.tradingview.com/pine-script-reference/v6/#fun_Enterlongtradesonthreerisingbarsexitontwofallingbars.)

// Enter long trades on three rising bars; exit on two falling bars.
if ta.rising(close, 3)
    strategy.entry("Long", strategy.long)
if ta.falling(close, 2)
    strategy.close("Long")

## [// Calculate the average trade duration.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculatetheaveragetradeduration.)

// Calculate the average trade duration.
avgTradeDuration() =>
    sumTradeDuration = 0
    for i = 0 to strategy.closedtrades - 1
        sumTradeDuration += strategy.closedtrades.exit_time(i) - strategy.closedtrades.entry_time(i)
    result = nz(sumTradeDuration / strategy.closedtrades)

## [// Display average duration converted to seconds and formatted using 2 decimal points.](https://www.tradingview.com/pine-script-reference/v6/#fun_Displayaveragedurationconvertedtosecondsandformattedusing2decimalpoints.)

// Display average duration converted to seconds and formatted using 2 decimal points.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(avgTradeDuration() / 1000, "#.##") + " seconds")
Example

## [// Reopens a closed trade after X seconds.](https://www.tradingview.com/pine-script-reference/v6/#fun_ReopensaclosedtradeafterXseconds.)

// Reopens a closed trade after X seconds.
//@version=6
strategy("strategy.closedtrades.exit_time Example 2")

## [// Strategy calls to emulate a single long trade at the first bar.](https://www.tradingview.com/pine-script-reference/v6/#fun_Strategycallstoemulateasinglelongtradeatthefirstbar.)

// Strategy calls to emulate a single long trade at the first bar.
if bar_index == 0
    strategy.entry("Long", strategy.long)

## [reopenPositionAfter(timeSec) =>](https://www.tradingview.com/pine-script-reference/v6/#fun_reopenPositionAftertimeSec)

reopenPositionAfter(timeSec) =>
    if strategy.closedtrades > 0
        if time - strategy.closedtrades.exit_time(strategy.closedtrades - 1) >= timeSec * 1000
            strategy.entry("Long", strategy.long)

## [// Reopen last closed position after 120 sec.](https://www.tradingview.com/pine-script-reference/v6/#fun_Reopenlastclosedpositionafter120sec.)

// Reopen last closed position after 120 sec.
reopenPositionAfter(120)

## [if ta.change(strategy.opentrades) != 0](https://www.tradingview.com/pine-script-reference/v6/#fun_ifta.changestrategy.opentrades0)

if ta.change(strategy.opentrades) != 0
    strategy.exit("Long", stop = low *0.9, profit = high* 2.5)
See also
strategy.closedtrades.entry_time()
strategy.closedtrades.max_drawdown()

## [Returns the maximum drawdown of the closed trade, i.e., the maximum possible loss during the trade, expressed in strategy.account_currency.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthemaximumdrawdownoftheclosedtradei.e.themaximumpossiblelossduringthetradeexpressedinstrategy.account_currency.)

Returns the maximum drawdown of the closed trade, i.e., the maximum possible loss during the trade, expressed in strategy.account_currency.
Syntax
strategy.closedtrades.max_drawdown(trade_num) → series float
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("`strategy.closedtrades.max_drawdown` Example")

## [// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.](https://www.tradingview.com/pine-script-reference/v6/#fun_Strategycallstoenterlongtradesevery15barsandexitlongtradesevery20bars.)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

## [// Get the biggest max trade drawdown value from all of the closed trades.](https://www.tradingview.com/pine-script-reference/v6/#fun_Getthebiggestmaxtradedrawdownvaluefromalloftheclosedtrades.)

// Get the biggest max trade drawdown value from all of the closed trades.
maxTradeDrawDown() =>
    maxDrawdown = 0.0
    for tradeNo = 0 to strategy.closedtrades - 1
        maxDrawdown := math.max(maxDrawdown, strategy.closedtrades.max_drawdown(tradeNo))
    result = maxDrawdown

## [plot(maxTradeDrawDown(), "Biggest max drawdown")](https://www.tradingview.com/pine-script-reference/v6/#fun_plotmaxTradeDrawDownBiggestmaxdrawdown)

plot(maxTradeDrawDown(), "Biggest max drawdown")
Remarks
The function returns na if trade_num is not in the range: 0 to strategy.closedtrades - 1.
See also
strategy.opentrades.max_drawdown()
strategy.max_drawdown
strategy.closedtrades.max_drawdown_percent()

## [Returns the maximum drawdown of the closed trade, i.e., the maximum possible loss during the trade, expressed as a percentage and calculated by formula: Lowest Value During Trade / (Entry Price x Quantity) * 100.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthemaximumdrawdownoftheclosedtradei.e.themaximumpossiblelossduringthetradeexpressedasapercentageandcalculatedbyformulaLowestValueDuringTradeEntryPricexQuantity100.)

Returns the maximum drawdown of the closed trade, i.e., the maximum possible loss during the trade, expressed as a percentage and calculated by formula: Lowest Value During Trade / (Entry Price x Quantity) * 100.
Syntax
strategy.closedtrades.max_drawdown_percent(trade_num) → series float
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
See also
strategy.closedtrades.max_drawdown()
strategy.max_drawdown
strategy.closedtrades.max_runup()

## [Returns the maximum run up of the closed trade, i.e., the maximum possible profit during the trade, expressed in strategy.account_currency.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthemaximumrunupoftheclosedtradei.e.themaximumpossibleprofitduringthetradeexpressedinstrategy.account_currency.)

Returns the maximum run up of the closed trade, i.e., the maximum possible profit during the trade, expressed in strategy.account_currency.
Syntax
strategy.closedtrades.max_runup(trade_num) → series float
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("`strategy.closedtrades.max_runup` Example")

## [// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.](https://www.tradingview.com/pine-script-reference/v6/#fun_Strategycallstoenterlongtradesevery15barsandexitlongtradesevery20bars.)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

## [// Get the biggest max trade runup value from all of the closed trades.](https://www.tradingview.com/pine-script-reference/v6/#fun_Getthebiggestmaxtraderunupvaluefromalloftheclosedtrades.)

// Get the biggest max trade runup value from all of the closed trades.
maxTradeRunUp() =>
    maxRunup = 0.0
    for tradeNo = 0 to strategy.closedtrades - 1
        maxRunup := math.max(maxRunup, strategy.closedtrades.max_runup(tradeNo))
    result = maxRunup

## [plot(maxTradeRunUp(), "Max trade runup")](https://www.tradingview.com/pine-script-reference/v6/#fun_plotmaxTradeRunUpMaxtraderunup)

plot(maxTradeRunUp(), "Max trade runup")
See also
strategy.opentrades.max_runup()
strategy.max_runup
strategy.closedtrades.max_runup_percent()

## [Returns the maximum run-up of the closed trade, i.e., the maximum possible profit during the trade, expressed as a percentage and calculated by formula: Highest Value During Trade / (Entry Price x Quantity) * 100.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthemaximumrun-upoftheclosedtradei.e.themaximumpossibleprofitduringthetradeexpressedasapercentageandcalculatedbyformulaHighestValueDuringTradeEntryPricexQuantity100.)

Returns the maximum run-up of the closed trade, i.e., the maximum possible profit during the trade, expressed as a percentage and calculated by formula: Highest Value During Trade / (Entry Price x Quantity) * 100.
Syntax
strategy.closedtrades.max_runup_percent(trade_num) → series float
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
See also
strategy.closedtrades.max_runup()
strategy.max_runup
strategy.closedtrades.profit()

## [Returns the profit/loss of the closed trade in the strategy's account currency, reduced by the trade's commissions. A positive returned value represents a profit, and a negative value represents a loss.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnstheprofitlossoftheclosedtradeinthestrategysaccountcurrencyreducedbythetradescommissions.Apositivereturnedvaluerepresentsaprofitandanegativevaluerepresentsaloss.)

Returns the profit/loss of the closed trade in the strategy's account currency, reduced by the trade's commissions. A positive returned value represents a profit, and a negative value represents a loss.
Syntax
strategy.closedtrades.profit(trade_num) → series float
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("`strategy.closedtrades.profit()` example")

## [// Enter a long trade every 15 bars, and close a long trade every 20 bars.](https://www.tradingview.com/pine-script-reference/v6/#fun_Enteralongtradeevery15barsandclosealongtradeevery20bars.)

// Enter a long trade every 15 bars, and close a long trade every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

## [//@function Calculates the average gross profit from all available closed trades.](https://www.tradingview.com/pine-script-reference/v6/#fun_functionCalculatestheaveragegrossprofitfromallavailableclosedtrades.)

//@function Calculates the average gross profit from all available closed trades.
avgGrossProfit() =>
    var float result = 0.0
    if result == 0.0 or strategy.closedtrades > strategy.closedtrades[1]
        float sumGrossProfit = 0.0
        for tradeNo = 0 to strategy.closedtrades - 1
            sumGrossProfit += strategy.closedtrades.profit(tradeNo)
        result := nz(sumGrossProfit / strategy.closedtrades)
    result

## [plot(avgGrossProfit(), "Average gross profit")](https://www.tradingview.com/pine-script-reference/v6/#fun_plotavgGrossProfitAveragegrossprofit)

plot(avgGrossProfit(), "Average gross profit")
See also
strategy.account_currency
strategy.opentrades.profit()
strategy.closedtrades.commission()
strategy.closedtrades.profit_percent()

## [Returns the profit/loss value of the closed trade, expressed as a percentage. Losses are expressed as negative values.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnstheprofitlossvalueoftheclosedtradeexpressedasapercentage.Lossesareexpressedasnegativevalues.)

Returns the profit/loss value of the closed trade, expressed as a percentage. Losses are expressed as negative values.
Syntax
strategy.closedtrades.profit_percent(trade_num) → series float
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
See also
strategy.closedtrades.profit()
strategy.closedtrades.size()

## [Returns the direction and the number of contracts traded in the closed trade. If the value is > 0, the market position was long. If the value is < 0, the market position was short.](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnsthedirectionandthenumberofcontractstradedintheclosedtrade.Ifthevalueis0themarketpositionwaslong.Ifthevalueis0themarketpositionwasshort.)

Returns the direction and the number of contracts traded in the closed trade. If the value is > 0, the market position was long. If the value is < 0, the market position was short.
Syntax
strategy.closedtrades.size(trade_num) → series float
Arguments
trade_num (series int) The trade number of the closed trade. The number of the first trade is zero.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("`strategy.closedtrades.size` Example 1")

## [// We calculate the max amt of shares we can buy.](https://www.tradingview.com/pine-script-reference/v6/#fun_Wecalculatethemaxamtofshareswecanbuy.)

// We calculate the max amt of shares we can buy.
amtShares = math.floor(strategy.equity / close)
// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long, qty = amtShares)
if bar_index % 20 == 0
    strategy.close("Long")

## [// Plot the number of contracts traded in the last closed trade.](https://www.tradingview.com/pine-script-reference/v6/#fun_Plotthenumberofcontractstradedinthelastclosedtrade.)

// Plot the number of contracts traded in the last closed trade.
plot(strategy.closedtrades.size(strategy.closedtrades - 1), "Number of contracts traded")
Example

## [// Calculates the average profit percentage for all closed trades.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculatestheaverageprofitpercentageforallclosedtrades.)

// Calculates the average profit percentage for all closed trades.
//@version=6
strategy("`strategy.closedtrades.size` Example 2")

## [// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.](https://www.tradingview.com/pine-script-reference/v6/#fun_Strategycallstoenterlongtradesevery15barsandexitlongtradesevery20bars.)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

## [// Calculate profit for both closed trades.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculateprofitforbothclosedtrades.)

// Calculate profit for both closed trades.
profitPct = 0.0
for tradeNo = 0 to strategy.closedtrades - 1
    entryP = strategy.closedtrades.entry_price(tradeNo)
    exitP = strategy.closedtrades.exit_price(tradeNo)
    profitPct += (exitP - entryP) / entryP *strategy.closedtrades.size(tradeNo)* 100

## [// Calculate average profit percent for both closed trades.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculateaverageprofitpercentforbothclosedtrades.)

// Calculate average profit percent for both closed trades.
avgProfitPct = nz(profitPct / strategy.closedtrades)

## [plot(avgProfitPct)](https://www.tradingview.com/pine-script-reference/v6/#fun_plotavgProfitPct)

plot(avgProfitPct)
See also
strategy.opentrades.size()
strategy.position_size
strategy.closedtrades
strategy.opentrades
strategy.convert_to_account()

## [Converts the value from the currency that the symbol on the chart is traded in (syminfo.currency) to the currency used by the strategy (strategy.account_currency).](https://www.tradingview.com/pine-script-reference/v6/#fun_Convertsthevaluefromthecurrencythatthesymbolonthechartistradedinsyminfo.currencytothecurrencyusedbythestrategystrategy.account_currency.)

Converts the value from the currency that the symbol on the chart is traded in (syminfo.currency) to the currency used by the strategy (strategy.account_currency).
Syntax
strategy.convert_to_account(value) → series float
Arguments
value (series int/float) The value to be converted.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("`strategy.convert_to_account` Example 1", currency = currency.EUR)

## [plot(close, "Close price using default currency")](https://www.tradingview.com/pine-script-reference/v6/#fun_plotcloseClosepriceusingdefaultcurrency)

plot(close, "Close price using default currency")
plot(strategy.convert_to_account(close), "Close price converted to strategy currency")
Example

## [// Calculates the "Buy and hold return" using your account's currency.](https://www.tradingview.com/pine-script-reference/v6/#fun_CalculatestheBuyandholdreturnusingyouraccountscurrency.)

// Calculates the "Buy and hold return" using your account's currency.
//@version=6
strategy("`strategy.convert_to_account` Example 2", currency = currency.EUR)

## [dateInput = input.time(timestamp("20 Jul 2021 00:00 +0300"), "From Date", confirm = true)](https://www.tradingview.com/pine-script-reference/v6/#fun_dateInputinput.timetimestamp20Jul202100000300FromDateconfirmtrue)

dateInput = input.time(timestamp("20 Jul 2021 00:00 +0300"), "From Date", confirm = true)

## [buyAndHoldReturnPct(fromDate) =>](https://www.tradingview.com/pine-script-reference/v6/#fun_buyAndHoldReturnPctfromDate)

buyAndHoldReturnPct(fromDate) =>
    if time >= fromDate
        money = close *syminfo.pointvalue
        var initialBal = strategy.convert_to_account(money)
        (strategy.convert_to_account(money) - initialBal) / initialBal* 100

## [plot(buyAndHoldReturnPct(dateInput))](https://www.tradingview.com/pine-script-reference/v6/#fun_plotbuyAndHoldReturnPctdateInput)

plot(buyAndHoldReturnPct(dateInput))
See also
strategy()
strategy.convert_to_symbol()
strategy.convert_to_symbol()

## [Converts the value from the currency used by the strategy (strategy.account_currency) to the currency that the symbol on the chart is traded in (syminfo.currency).](https://www.tradingview.com/pine-script-reference/v6/#fun_Convertsthevaluefromthecurrencyusedbythestrategystrategy.account_currencytothecurrencythatthesymbolonthechartistradedinsyminfo.currency.)

Converts the value from the currency used by the strategy (strategy.account_currency) to the currency that the symbol on the chart is traded in (syminfo.currency).
Syntax
strategy.convert_to_symbol(value) → series float
Arguments
value (series int/float) The value to be converted.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("`strategy.convert_to_symbol` Example", currency = currency.EUR)

## [// Calculate the max qty we can buy using current chart's currency.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculatethemaxqtywecanbuyusingcurrentchartscurrency.)

// Calculate the max qty we can buy using current chart's currency.
calcContracts(accountMoney) =>
    math.floor(strategy.convert_to_symbol(accountMoney) / syminfo.pointvalue / close)

## [// Return max qty we can buy using 300 euros](https://www.tradingview.com/pine-script-reference/v6/#fun_Returnmaxqtywecanbuyusing300euros)

// Return max qty we can buy using 300 euros
qt = calcContracts(300)

## [// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars using our custom qty.](https://www.tradingview.com/pine-script-reference/v6/#fun_Strategycallstoenterlongtradesevery15barsandexitlongtradesevery20barsusingourcustomqty.)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars using our custom qty.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long, qty = qt)
if bar_index % 20 == 0
    strategy.close("Long")
See also
strategy()
strategy.convert_to_account()
strategy.default_entry_qty()

## [Calculates the default quantity, in units, of an entry order from strategy.entry() or strategy.order() if it were to fill at the specified fill_price value. The calculation depends on several strategy properties, including default_qty_type, default_qty_value, currency, and other parameters in the strategy() function and their representation in the "Properties" tab of the strategy's settings.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculatesthedefaultquantityinunitsofanentryorderfromstrategy.entryorstrategy.orderifitweretofillatthespecifiedfill_pricevalue.Thecalculationdependsonseveralstrategypropertiesincludingdefault_qty_typedefault_qty_valuecurrencyandotherparametersinthestrategyfunctionandtheirrepresentationinthePropertiestabofthestrategyssettings.)

Calculates the default quantity, in units, of an entry order from strategy.entry() or strategy.order() if it were to fill at the specified fill_price value. The calculation depends on several strategy properties, including default_qty_type, default_qty_value, currency, and other parameters in the strategy() function and their representation in the "Properties" tab of the strategy's settings.
Syntax
strategy.default_entry_qty(fill_price) → series float
Arguments
fill_price (series int/float) The fill price for which to calculate the default order quantity.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("Supertrend Strategy", overlay = true, default_qty_type = strategy.percent_of_equity, default_qty_value = 15)

## [//@variable The length of the ATR calculation.](https://www.tradingview.com/pine-script-reference/v6/#fun_variableThelengthoftheATRcalculation.)

//@variable The length of the ATR calculation.
atrPeriod = input(10, "ATR Length")
//@variable The ATR multiplier.
factor = input.float(3.0, "Factor", step = 0.01)
//@variable The tick offset of the stop order.
stopOffsetInput = input.int(100, "Tick offset for entry stop")

## [// Get the direction of the SuperTrend.](https://www.tradingview.com/pine-script-reference/v6/#fun_GetthedirectionoftheSuperTrend.)

// Get the direction of the SuperTrend.
[_, direction] = ta.supertrend(factor, atrPeriod)

## [if ta.change(direction) < 0](https://www.tradingview.com/pine-script-reference/v6/#fun_ifta.changedirection0)

if ta.change(direction) < 0
    //@variable The stop price of the entry order.
    stopPrice = close + syminfo.mintick * stopOffsetInput
    //@variable The expected default fill quantity at the `stopPrice`. This value may not reflect actual qty of the filled order, because fill price may be different.
    calculatedQty = strategy.default_entry_qty(stopPrice)
    strategy.entry("My Long Entry Id", strategy.long, stop = stopPrice)
    label.new(bar_index, stopPrice, str.format("Stop set at {0}\nExpected qty at {0}: {1}", math.round_to_mintick(stopPrice), calculatedQty))

## [if ta.change(direction) > 0](https://www.tradingview.com/pine-script-reference/v6/#fun_ifta.changedirection0)

if ta.change(direction) > 0
    strategy.close_all()
Remarks
This function does not consider open positions simulated by a strategy. For example, if a strategy script has an open position from a long order with a qty of 10 units, using the strategy.entry() function to simulate a short order with a qty of 5 will prompt the script to sell 15 units to reverse the position. This function will still return 5 in such a case since it doesn't consider an open trade.
This value represents the default calculated quantity of an order.
Order placement commands can override the default value by explicitly passing a new qty value in the function call.
strategy.entry()

## [Creates a new order to open or add to a position. If an unfilled order with the same id exists, a call to this command modifies that order.](https://www.tradingview.com/pine-script-reference/v6/#fun_Createsanewordertoopenoraddtoaposition.Ifanunfilledorderwiththesameidexistsacalltothiscommandmodifiesthatorder.)

Creates a new order to open or add to a position. If an unfilled order with the same id exists, a call to this command modifies that order.
The resulting order's type depends on the limit and stop parameters. If the call does not contain limit or stop arguments, it creates a market order that executes on the next tick. If the call specifies a limit value but no stop value, it places a limit order that executes after the market price reaches the limit value or a better price (lower for buy orders and higher for sell orders). If the call specifies a stop value but no limit value, it places a stop order that executes after the market price reaches the stop value or a worse price (higher for buy orders and lower for sell orders). If the call contains limit and stop arguments, it creates a stop-limit order, which generates a limit order at the limit price only after the market price reaches the stop value or a worse price.
Orders from this command, unlike those from strategy.order(), are affected by the pyramiding parameter of the strategy() declaration statement. Pyramiding specifies the number of concurrent open entries allowed per position. For example, with pyramiding = 3, the strategy can have up to three open trades, and the command cannot create orders to open additional trades until at least one existing trade closes.
By default, when a strategy executes an order from this command in the opposite direction of the current market position, it reverses that position. For example, if there is an open long position of five shares, an order from this command with a qty of 5 and a direction of strategy.short triggers the sale of 10 shares to close the long position and open a new five-share short position. Users can change this behavior by specifying an allowed direction with the strategy.risk_allow_entry_in() function.
Learn more
Syntax
strategy.entry(id, direction, qty, limit, stop, oca_name, oca_type, comment, alert_message, disable_alert) → void
Arguments
id (series string) The identifier of the order, which corresponds to an entry ID in the strategy's trades after the order fills. If the strategy opens a new position after filling the order, the order's ID becomes the strategy.position_entry_name value. Strategy commands can reference the order ID to cancel or modify pending orders and generate exit orders for specific open trades. The Strategy Tester and the chart display the order ID unless the command specifies a comment value.
direction (series strategy_direction) The direction of the trade. Possible values: strategy.long for a long trade, strategy.short for a short one.
qty (series int/float) Optional. The number of contracts/shares/lots/units in the resulting open trade when the order fills. The default is na, which means that the command uses the default_qty_type and default_qty_value parameters of the strategy() declaration statement to determine the quantity.
limit (series int/float) Optional. The limit price of the order. If specified, the command creates a limit or stop-limit order, depending on whether the stop value is also specified. The default is na, which means the resulting order is not of the limit or stop-limit type.
stop (series int/float) Optional. The stop price of the order. If specified, the command creates a stop or stop-limit order, depending on whether the limit value is also specified. The default is na, which means the resulting order is not of the stop or stop-limit type.
oca_name (series string) Optional. The name of the order's One-Cancels-All (OCA) group. When a pending order with the same oca_name and oca_type parameters executes, that order affects all unfilled orders in the group. The default is an empty string, which means the order does not belong to an OCA group.
oca_type (input string) Optional. Specifies how an unfilled order behaves when another pending order with the same oca_name and oca_type values executes. Possible values: strategy.oca.cancel, strategy.oca.reduce, strategy.oca.none. The default is strategy.oca.none.
comment (series string) Optional. Additional notes on the filled order. If the value is not an empty string, the Strategy Tester and the chart show this text for the order instead of the specified id. The default is an empty string.
alert_message (series string) Optional. Custom text for the alert that fires when an order fills. If the "Message" field of the "Create Alert" dialog box contains the {{strategy.order.alert_message}} placeholder, the alert message replaces the placeholder with this text. The default is an empty string.
disable_alert (series bool) Optional. If true when the command creates an order, the strategy does not trigger an alert when that order fills. This parameter accepts a "series" value, meaning users can control which orders trigger alerts when they execute. The default is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("Market order strategy", overlay = true)

## [// Calculate a 14-bar and 28-bar moving average of `close` prices.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculatea14-barand28-barmovingaverageofcloseprices.)

// Calculate a 14-bar and 28-bar moving average of `close` prices.
float sma14 = ta.sma(close, 14)
float sma28 = ta.sma(close, 28)

## [// Place a market order to close the short trade and enter a long position when `sma14` crosses over `sma28`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Placeamarketordertoclosetheshorttradeandenteralongpositionwhensma14crossesoversma28.)

// Place a market order to close the short trade and enter a long position when `sma14` crosses over `sma28`.
if ta.crossover(sma14, sma28)
    strategy.entry("My Long Entry ID", strategy.long)

## [// Place a market order to close the long trade and enter a short position when `sma14` crosses under `sma28`.](https://www.tradingview.com/pine-script-reference/v6/#fun_Placeamarketordertoclosethelongtradeandenterashortpositionwhensma14crossesundersma28.)

// Place a market order to close the long trade and enter a short position when `sma14` crosses under `sma28`.
if ta.crossunder(sma14, sma28)
    strategy.entry("My Short Entry ID", strategy.short)
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("Limit order strategy", overlay=true, margin_long=100, margin_short=100)

## [//@variable The distance from the `close` price for each limit order.](https://www.tradingview.com/pine-script-reference/v6/#fun_variableThedistancefromtheclosepriceforeachlimitorder.)

//@variable The distance from the `close` price for each limit order.
float limitOffsetInput = input.int(100, "Limit offset, in ticks", 1) * syminfo.mintick

## [//@function Draws a label and line at the specified `price` to visualize a limit order's level.](https://www.tradingview.com/pine-script-reference/v6/#fun_functionDrawsalabelandlineatthespecifiedpricetovisualizealimitorderslevel.)

//@function Draws a label and line at the specified `price` to visualize a limit order's level.
drawLimit(float price, bool isLong) =>
    color col = isLong ? color.blue : color.red
    label.new(
         bar_index, price, (isLong ? "Long" : "Short") + " limit order created",
         style = label.style_label_right, color = col, textcolor = color.white
     )
    line.new(bar_index, price, bar_index + 1, price, extend = extend.right, style = line.style_dashed, color = col)

## [//@function Stops the `l` line from extending further.](https://www.tradingview.com/pine-script-reference/v6/#fun_functionStopsthellinefromextendingfurther.)

//@function Stops the `l` line from extending further.
method stopExtend(line l) =>
    l.set_x2(bar_index)
    l.set_extend(extend.none)

## [// Initialize two `line` variables to reference limit line IDs.](https://www.tradingview.com/pine-script-reference/v6/#fun_InitializetwolinevariablestoreferencelimitlineIDs.)

// Initialize two `line` variables to reference limit line IDs.
var line longLimit  = na
var line shortLimit = na

## [// Calculate a 14-bar and 28-bar moving average of `close` prices.](https://www.tradingview.com/pine-script-reference/v6/#fun_Calculatea14-barand28-barmovingaverageofcloseprices.)

// Calculate a 14-bar and 28-bar moving average of `close` prices.
float sma14 = ta.sma(close, 14)
float sma28 = ta.sma(close, 28)

## [if ta.crossover(sma14, sma28)](https://www.tradingview.com/pine-script-reference/v6/#fun_ifta.crossoversma14sma28)

if ta.crossover(sma14, sma28)
    // Cancel any unfilled sell orders with the specified ID.
    strategy.cancel("My Short Entry ID")
    //@variable The limit price level. Its value is `limitOffsetInput` ticks below the current `close`.
    float limitLevel = close - limitOffsetInput
    // Place a long limit order to close the short trade and enter a long position at the `limitLevel`.
    strategy.entry("My Long Entry ID", strategy.long, limit = limitLevel)
    // Make new drawings for the long limit and stop extending the `shortLimit` line.
    longLimit := drawLimit(limitLevel, isLong = true)
    shortLimit.stopExtend()

## [if ta.crossunder(sma14, sma28)](https://www.tradingview.com/pine-script-reference/v6/#fun_ifta.crossundersma14sma28)

if ta.crossunder(sma14, sma28)
    // Cancel any unfilled buy orders with the specified ID.
    strategy.cancel("My Long Entry ID")
    //@variable The limit price level. Its value is `limitOffsetInput` ticks above the current `close`.
    float limitLevel = close + limitOffsetInput
    // Place a short limit order to close the long trade and enter a short position at the `limitLevel`.
    strategy.entry("My Short Entry ID", strategy.short, limit = limitLevel)
    // Make new drawings for the short limit and stop extending the `shortLimit` line.
    shortLimit := drawLimit(limitLevel, isLong = false)
    longLimit.stopExtend()
strategy.exit()

## [Creates price-based orders to exit from an open position. If unfilled exit orders with the same id exist, calls to this command modify those orders. This command can generate more than one type of exit order, depending on the specified parameters. However, it does not create market orders. To exit from a position with a market order, use strategy.close() or strategy.close_all().](https://www.tradingview.com/pine-script-reference/v6/#fun_Createsprice-basedorderstoexitfromanopenposition.Ifunfilledexitorderswiththesameidexistcallstothiscommandmodifythoseorders.Thiscommandcangeneratemorethanonetypeofexitorderdependingonthespecifiedparameters.Howeveritdoesnotcreatemarketorders.Toexitfromapositionwithamarketorderusestrategy.closeorstrategy.close_all.)

Creates price-based orders to exit from an open position. If unfilled exit orders with the same id exist, calls to this command modify those orders. This command can generate more than one type of exit order, depending on the specified parameters. However, it does not create market orders. To exit from a position with a market order, use strategy.close() or strategy.close_all().
If a call to this command contains a profit or limit argument, it creates take-profit orders to exit from applicable trades at the determined price levels or better values (higher for long trades and lower for short ones). If the call contains loss or stop arguments, it creates stop-loss orders to exit from applicable trades at the determined levels or worse values (lower for long trades and higher for short ones). Calling this command with profit or limit and loss or stop arguments creates an order bracket with both order types.
This command can create trailing stop orders when its call specifies a trail_price or trail_points argument and a trail_offset argument. A trailing stop order activates when the price moves trail_points ticks past the entry price or touches the trail_price level. Once activated, the stop follows trail_offset ticks behind the market price each time the trade's profit reaches a new high. The stop does not move when the trade does not achieve a new best value.
Each call to this command reserves a portion of the position to close until the strategy fills or cancels its orders. For example, if there is an open position of 50 contracts and a strategy.exit() call specifies a qty of 20, that call's orders reserve 20 contracts out of the position. A second call can close a maximum of 30 contracts, even if its qty is 50 and one of its orders executes first. This behavior does not affect the orders from other commands, such as strategy.close() or strategy.order().
If a call to this command occurs before a created entry order's execution, the strategy waits and does not create the exit orders until after the entry order executes.
Learn more
Syntax
strategy.exit(id, from_entry, qty, qty_percent, profit, limit, loss, stop, trail_price, trail_points, trail_offset, oca_name, comment, comment_profit, comment_loss, comment_trailing, alert_message, alert_profit, alert_loss, alert_trailing, disable_alert) → void
Arguments
id (series string) The identifier of the orders, which corresponds to an exit ID in the strategy's trades after an order fills. Strategy commands can reference the order ID to cancel or modify pending exit orders. The Strategy Tester and the chart display the order ID unless the command includes a comment*argument that applies to the filled order.
from_entry (series string) Optional. The entry order ID of the trade to exit from. If there is more than one open trade with the specified entry ID, the command generates exit orders for all the entries from before or at the time of the call. The default is an empty string, which means the command generates exit orders for all open trades until the position closes.
qty (series int/float) Optional. The number of contracts/lots/shares/units to close when an exit order fills. If specified, the command uses this value instead of qty_percent to determine the order size. The exit orders reserve this quantity from the position, meaning other calls to this command cannot close this portion until the strategy fills or cancels those orders. The default is na, which means the order size depends on the qty_percent value.
qty_percent (series int/float) Optional. A value between 0 and 100 representing the percentage of the open trade quantity to close when an exit order fills. The exit orders reserve this percentage from the applicable open trades, meaning other calls to this command cannot close this portion until the strategy fills or cancels those orders. The percentage calculation depends on the total size of the applicable open trades without considering the reserved amount from other strategy.exit() calls. The command ignores this parameter if the qty value is not na. The default is 100.
profit (series int/float) Optional. The take-profit distance, expressed in ticks. If specified, the command creates a limit order to exit the trade profit ticks away from the entry price in the favorable direction. The order executes at the calculated price or a better value. If this parameter and limit are not na, the command places a take-profit order only at the price level expected to trigger an exit first. The default is na.
limit (series int/float) Optional. The take-profit price. If this parameter and profit are not na, the command places a take-profit order only at the price level expected to trigger an exit first. The default is na.
loss (series int/float) Optional. The stop-loss distance, expressed in ticks. If specified, the command creates a stop order to exit the trade loss ticks away from the entry price in the unfavorable direction. The order executes at the calculated price or a worse value. If this parameter and stop are not na, the command places a stop-loss order only at the price level expected to trigger an exit first. The default is na.
stop (series int/float) Optional. The stop-loss price. If this parameter and loss are not na, the command places a stop-loss order only at the price level expected to trigger an exit first. The default is na.
trail_price (series int/float) Optional. The price of the trailing stop activation level. If the value is more favorable than the entry price, the command creates a trailing stop when the market price reaches that value. If less favorable than the entry price, the command creates the trailing stop immediately when the current market price is equal to or more favorable than the value. If this parameter and trail_points are not na, the command sets the activation level using the value expected to activate the stop first. The default is na.
trail_points (series int/float) Optional. The trailing stop activation distance, expressed in ticks. If the value is positive, the command creates a trailing stop order when the market price moves trail_points ticks away from the trade's entry price in the favorable direction. If the value is negative, the command creates the trailing stop immediately when the market price is equal to or more favorable than the level trail_points ticks away from the entry price in the unfavorable direction. The default is na.
trail_offset (series int/float) Optional. The trailing stop offset. When the market price reaches the activation level determined by the trail_price or trail_points parameter, or exceeds the level in the favorable direction, the command creates a trailing stop with an initial value trail_offset ticks away from that level in the unfavorable direction. After activation, the trailing stop moves toward the market price each time the trade's profit reaches a better value, maintaining the specified distance behind the best price. The default is na.
oca_name (series string) Optional. The name of the One-Cancels-All (OCA) group that the command's take-profit, stop-loss, and trailing stop orders belong to. All orders from this command are of the strategy.oca.reduce OCA type. When an order of this OCA type with the same oca_name executes, the strategy reduces the sizes of other unfilled orders in the OCA group by the filled quantity. The default is an empty string, which means the strategy assigns the OCA name automatically, and the resulting orders cannot reduce or be reduced by the orders from other commands.
comment (series string) Optional. Additional notes on the filled order. If the value is not an empty string, the Strategy Tester and the chart show this text for the order instead of the specified id. The command ignores this value if the call includes an argument for a comment_* parameter that applies to the order. The default is an empty string.
comment_profit (series string) Optional. Additional notes on the filled order. If the value is not an empty string, the Strategy Tester and the chart show this text for the order instead of the specified id or comment. This comment applies only to the command's take-profit orders created using the profit or limit parameter. The default is an empty string.
comment_loss (series string) Optional. Additional notes on the filled order. If the value is not an empty string, the Strategy Tester and the chart show this text for the order instead of the specified id or comment. This comment applies only to the command's stop-loss orders created using the loss or stop parameter. The default is an empty string.
comment_trailing (series string) Optional. Additional notes on the filled order. If the value is not an empty string, the Strategy Tester and the chart show this text for the order instead of the specified id or comment. This comment applies only to the command's trailing stop orders created using the trail_price or trail_points and trail_offset parameters. The default is an empty string.
alert_message (series string) Optional. Custom text for the alert that fires when an order fills. If the "Message" field of the "Create Alert" dialog box contains the {{strategy.order.alert_message}} placeholder, the alert message replaces the placeholder with this text. The command ignores this value if the call includes an argument for the other alert_* parameter that applies to the order. The default is an empty string.
alert_profit (series string) Optional. Custom text for the alert that fires when an order fills. If the "Message" field of the "Create Alert" dialog box contains the {{strategy.order.alert_message}} placeholder, the alert message replaces the placeholder with this text. This message applies only to the command's take-profit orders created using the profit or limit parameter. The default is an empty string.
alert_loss (series string) Optional. Custom text for the alert that fires when an order fills. If the "Message" field of the "Create Alert" dialog box contains the {{strategy.order.alert_message}} placeholder, the alert message replaces the placeholder with this text. This message applies only to the command's stop-loss orders created using the loss or stop parameter. The default is an empty string.
alert_trailing (series string) Optional. Custom text for the alert that fires when an order fills. If the "Message" field of the "Create Alert" dialog box contains the {{strategy.order.alert_message}} placeholder, the alert message replaces the placeholder with this text. This message applies only to the command's trailing stop orders created using the trail_price or trail_points and trail_offset parameters. The default is an empty string.
disable_alert (series bool) Optional. If true when the command creates an order, the strategy does not trigger an alert when that order fills. This parameter accepts a "series" value, meaning users can control which orders trigger alerts when they execute. The default is false.
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#fun_version6)

//@version=6
strategy("Exit bracket strategy", overlay = true)

## [// Inputs that define the profit and loss amount of each trade as a tick distance from the entry price.](https://www.tradingview.com/pine-script-reference/v6/#fun_Inputsthatdefinetheprofitandlossamountofeachtradeasatickdistancefromtheentryprice.)

// Inputs that define the profit and loss amount of each trade as a tick distance from the entry price.
int profitDistanceInput = input.int(100, "Profit distance, in ticks", 1)
int lossDistanceInput   = input.int(100, "Loss distance, in ticks", 1)

## [//](https://www.tradingview.com/pine-script-reference/v6/#fun_)

//
