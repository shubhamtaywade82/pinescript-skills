# Methods

Methods are specialized functions that attach to a type, enabling method-chaining syntax like `object.methodName()`.

### Rules
- Must use the `method` keyword instead of `function`.
- The first parameter must be the type it attaches to.

```pinescript
//@version=6
indicator("Methods Demo")

type BarData
    float o
    float c

// Bind method to BarData
method spread(BarData bd) =>
    math.abs(bd.c - bd.o)

var currentBar = BarData.new(open, close)
float barSpread = currentBar.spread()

plot(barSpread)
```

See also:
- [Objects (UDTs)](objects.md)
