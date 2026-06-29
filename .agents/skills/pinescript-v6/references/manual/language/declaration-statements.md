# Declaration Statements

Every script must contain exactly one declaration statement in the global scope:
- `indicator(...)`: Creates a study or technical analysis indicator.
- `strategy(...)`: Creates a backtestable trading strategy.
- `library(...)`: Creates a library of shared functions, UDTs, or enums.

### Options
- `title`: The script name.
- `overlay`: If `true`, the output is drawn over the price panel.
- `format`: Format of the display values (e.g., `format.price`, `format.percent`).

```pinescript
//@version=6
indicator("Declaration Demo", overlay=true, format=format.price, precision=2)
plot(close)
```

See also:
- [Script Structure](script-structure.md)
