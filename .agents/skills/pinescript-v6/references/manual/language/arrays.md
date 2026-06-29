# Arrays

Arrays are ordered collections of variables of the same type.

### Declaring and Manipulating Arrays
```pinescript
//@version=6
indicator("Arrays Demo")

// Create a float array
var float[] priceHistory = array.new<float>(0)

// Add element
priceHistory.push(close)

// Access element
float firstPrice = priceHistory.size() > 0 ? priceHistory.get(0) : na
plot(firstPrice)
```

See also:
- [Collections Guide](../../collections.md)
- [Matrices](matrices.md)
- [Maps](maps.md)
