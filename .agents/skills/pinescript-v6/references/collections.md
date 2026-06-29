# Pine Script® v6 Collections, UDTs, and Methods

This guide explains how to define, manage, and use custom data structures, object methods, and collections (arrays, maps, matrices) in Pine Script® v6.

## 1. User-Defined Types (UDTs)

User-Defined Types let you group related fields into custom objects.

### Definition and Instantiation
Define a UDT using the `type` keyword. Fields can be of any type, including primitive types, arrays, maps, or other UDTs.
```pinescript
// 1. Defining the Type
type TradeSignal
    int barIndex
    float price
    string signalType // "Buy" or "Sell"

// 2. Creating an Object (Instantiating)
// Use the built-in `<TypeName>.new()` constructor
var mySignal = TradeSignal.new(bar_index, close, "Buy")
```

### Accessing Fields
Access the fields of an object using dot notation (`object.field`).
```pinescript
if barstate.islast
    log.info("Signal Type: {0}, Price: {1}", mySignal.signalType, mySignal.price)
```

---

## 2. History Referencing on Object Fields (CRITICAL)

In Pine Script v6, the history-referencing operator (`[]`) **cannot** be applied directly to a field of a UDT. The syntax `myObject.field[1]` will result in a compiler error.

You must access field history using one of the following two patterns:

### Pattern A: Reference Object History First
Wrap the object history lookup in parentheses, and then access the field.
```pinescript
// Correct way to get a field value from 1 bar ago
float pastPrice = (mySignal[1]).price
```

### Pattern B: Assign to a Local Variable First
Assign the field value to a local variable on each bar, then apply the history operator to that variable.
```pinescript
// Done on each bar
float currentPrice = mySignal.price

// Access history of the variable
float pastPrice = currentPrice[1]
```

---

## 3. Custom Methods

Methods associate a function with a type (UDTs or built-in types), allowing you to call them using dot notation: `object.methodName()`.

### Syntax Rules:
- Defined in the global scope using the `method` keyword.
- The first parameter of the method **must** specify the type it binds to.
- When calling the method, the object on which it is called is implicitly passed as the first argument.

```pinescript
// Bind method 'update' to type 'TradeSignal'
method update(TradeSignal ts, float newPrice) =>
    ts.price := newPrice
    ts.barIndex := bar_index

// Usage
mySignal.update(close)
```

---

## 4. Collections: Arrays, Maps, and Matrices

Pine Script v6 supports three collection types. All collections are referenced by their ID and support typed generics.

### Arrays
Ordered, index-based lists of elements.
```pinescript
// Create float array
float[] prices = array.new_float(0)
prices.push(1.23)
float firstPrice = prices.get(0)
```

### Maps
Unordered collections of key-value pairs.
- **Keys**: Must be a fundamental type (`int`, `float`, `bool`, `string`, `color`).
- **Values**: Can be any type, including UDTs.
- **Capacity**: Up to 50,000 items.
```pinescript
// Create map
var map<string, float> symMap = map.new<string, float>()
symMap.put("BTCUSD", 65000.0)
if symMap.contains("BTCUSD")
    float btcPrice = symMap.get("BTCUSD")
```

### Matrices
Two-dimensional tables of data.
```pinescript
// Create matrix (2 rows, 3 columns) initialized to 0.0
matrix<float> grid = matrix.new<float>(2, 3, 0.0)
grid.set(0, 0, 1.5)
```
