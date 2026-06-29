# Errors and Warnings Overview

Pine Script® uses three kinds of notifications to prevent and troubleshoot scripting errors:

### Runtime Errors
Runtime errors occur under specific conditions while a script runs on chart data. If a script encounters a runtime error:
- It stops execution and displays a red exclamation mark in the status line.
- Click the icon to view the exact error and the bar index where it occurred.
- You can trigger custom runtime errors using `runtime.error()`.

### Compilation Errors
Compilation errors occur at compile time *before* the script runs.
- They highlight the problematic line in red in the Pine Editor.
- They prevent compilation and must be resolved before the script can be loaded onto a chart.

### Compiler Warnings
Warnings also occur before the script runs.
- They do *not* prevent compilation but inform you of syntax or usage that might cause unintended behaviors (e.g. using deprecated functions or features).
- They highlight the code in orange. It is highly recommended to fix warnings to ensure script reliability.

See also:
- [CE10101](CE10101.md)
- [CW10003](CW10003.md)
