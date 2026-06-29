# Objects (UDTs)

User-Defined Types (UDTs) let you group related fields into custom object schemas.

### Syntax and Fields
```pinescript
//@version=6
indicator("Objects Demo", overlay=true)

// Define the UDT
type TradePosition
    int entryBar
    float entryPrice
    string direction

// Create instance
var myPosition = TradePosition.new(bar_index, close, "Long")

// Access fields
float entry = myPosition.entryPrice
```

> [!WARNING]
> History referencing on object fields must be done by indexing the object itself inside parentheses: `(myPosition[1]).entryPrice` or by copying the field to a local variable first. Applying the indexer directly to the field (`myPosition.entryPrice[1]`) will result in a compiler error.

See also:
- [Collections & UDTs](../../collections.md)
- [Methods](methods.md)
