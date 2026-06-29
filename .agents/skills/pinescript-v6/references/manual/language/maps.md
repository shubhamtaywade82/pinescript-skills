# Maps

Maps are key-value pairs where keys must be fundamental types (`int`, `float`, `bool`, `string`, `color`) and values can be any type.

```pinescript
//@version=6
indicator("Maps Demo")

// Create a map
var map<string, float> priceMap = map.new<string, float>()

// Store value
priceMap.put("LastClose", close)

// Retrieve value
float val = priceMap.get("LastClose")
plot(val)
```

See also:
- [Collections Guide](../../collections.md)
- [Arrays](arrays.md)
- [Matrices](matrices.md)
