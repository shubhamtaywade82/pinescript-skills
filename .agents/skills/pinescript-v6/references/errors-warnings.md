# Pine Script® v6 Errors and Warnings

Source: `User Manual > Errors and warnings > Overview`
See also: `Errors and warnings` index at `https://www.tradingview.com/pine-script-docs/errors/`

## Overview

### Introduction

Pine Script® uses _runtime errors_, _compilation errors_, and _compiler warnings_ to help prevent unintended or erroneous script behaviors:

- **Runtime errors** occur under specific conditions as a script runs on a dataset. If a script encounters a runtime error while it executes on the chart, it _stops_ executing and displays a red “exclamation point” icon in the _status line_. The user can click the icon to view the error message and the bar index on which the error occurred. Pine includes built-in runtime errors for invalid arguments in function calls, history-referencing operations with invalid offsets, and various other error conditions. Programmers can also define custom runtime errors for specific conditions in their scripts by using the [`runtime.error()`](https://www.tradingview.com/pine-script-reference/v6/#fun_runtime.error) function.

- **Compilation errors** occur at compile time, _before_ a script begins to run on a dataset. If a script contains _invalid syntax_ or other issues that _prevent_ compilation, the Pine Editor highlights the problematic code in red and displays an error message to indicate the cause. If the script is on the chart, users can also access the error message from the script’s status line.

- **Compiler warnings** also occur before a script begins to run. In contrast to compilation errors, these warnings inform users about syntax that does _not_ prevent compilation but can cause _unintended results_ or behaviors. They can also occur for other reasons, such as using deprecated features or old Pine Script versions. The Pine Editor highlights code that causes a warning in orange, then displays a message outlining the issue and potential solutions. We recommend following the suggestions in compiler warnings to help ensure that a script works as intended.

> You can also find information about other errors and warnings for specific features on the pages that describe those features in the User Manual. For example, the sections in the Type system page include details about common type-related errors.

## Error reference table

> This list is not exhaustive. New pages for other common errors and warnings will likely be added over time.

| Error/warning code | Message | Solution |
| :--- | :--- | :--- |
| `CE10101` | The condition of the “X” statement must evaluate to a “bool” value. | Use only expressions that return values of the `bool` type as the conditions in `if` and `switch` statements. |
| `CW10003` | The function “X” should be called on each calculation for consistency. It is recommended to extract the call from this scope. | The function call might cause unintended results when executing inside a conditional structure or loop because it relies on data from past bars. Move the call to the global scope, and outside conditional expressions, to ensure consistent history-based calculations. |
| `RE10139` | Memory limits exceeded. | There are multiple possible causes and solutions. A common cause is using `request.*()` calls to request large collections of data across bars. The usual solution for that case is to optimize the requests to return collection IDs only when necessary. Consult the error page to learn more. |
| `RE10143` | The requested historical offset (X) is beyond the historical buffer’s limit (Y). | This error occurs if a script references the history of a variable or expression from too many bars back. A typical solution is to use the `max_bars_back()` function to specify how many past bars of data to include in the historical buffer for the referenced series. |
