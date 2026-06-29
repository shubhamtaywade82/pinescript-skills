# Matrices

Matrices are two-dimensional collections of values of the same type.

```pinescript
//@version=6
indicator("Matrices Demo")

// Create matrix (2 rows, 2 columns, initial value 0.0)
var matrix<float> mat = matrix.new<float>(2, 2, 0.0)

// Set value at row 0, column 1
mat.set(0, 1, close)

// Get value
float val = mat.get(0, 1)
plot(val)
```

See also:
- [Collections Guide](../../collections.md)
- [Arrays](arrays.md)
- [Maps](maps.md)
