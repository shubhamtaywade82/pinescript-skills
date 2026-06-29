# Libraries

Pine Script® libraries are publications containing functions, user-defined types (UDTs), and enums that can be imported and reused in indicators, strategies, or other libraries. They help modularize code, preventing duplication across scripts.

All libraries are published open-source. Public scripts can only import public libraries. Private scripts can import either public or private libraries.

---

## Structure of a Library

A library starts with the `library()` declaration statement instead of `indicator()` or `strategy()`. It exposes structures via the `export` keyword.

```pinescript
//@version=6

// @description Provides calculations for the all-time high/low of values.
library("AllTimeHighLow", true)

// @function Calculates the all-time high of a series.
// @param val Series to use (`high` is used if no argument is supplied).
// @returns The all-time high for the series.
export hi(float val = high) =>
    var float ath = val
    ath := math.max(ath, val)

// @function Calculates the all-time low of a series.
// @param val Series to use (`low` is used if no argument is supplied).
// @returns The all-time low for the series.
export lo(float val = low) =>
    var float atl = val
    atl := math.min(atl, val)

// Demo code runs in global scope when loaded on a chart
plot(hi())
plot(lo())
```

### Compiler Annotations
We highly recommend documenting exports using compiler annotations (`// @description`, `// @function`, `// @param`, `// @returns`, `// @type`, `// @field`, `// @enum`). These generate default descriptions automatically when publishing.

---

## Library Function Constraints

Exported library functions have specific rules that do not apply to regular user-defined functions:
*   **Mandatory Export**: The `export` keyword is required before the function definition.
*   **Explicit Parameters**: Parameters must have explicit type keywords (e.g. `int x` or `float val`).
*   **No Global Variables**: They cannot reference variables in the library's global scope unless they are qualified as `const` (e.g., they cannot reference inputs or mutable global variables).
*   **No Inputs**: They cannot contain any `input.*()` calls.
*   **Dynamic Requests**: They can contain `request.*()` calls in local scopes, but the parameters of `request.*()` cannot depend on exported function arguments.
*   **Types Qualifiers**: Exported functions always return `simple` or `series` values. They cannot return `const` or `input` qualified values.

### Controlling Type Qualifiers (`simple` vs. `series`)
By default, the compiler evaluates library parameters as `series` unless code constraints require a weaker qualifier. 

If you need a function to yield a `simple` value (e.g., to generate a ticker symbol for `request.security()`), force the parameters to use the `simple` qualifier:

```pinescript
// Forces the function to return a "simple string"
export makeTickerid(simple string prefix, simple string ticker) =>
    prefix + ":" + ticker
```

> [!WARNING]
> If any part of the calculation inside a function involves `series` data, the return value becomes `series`, regardless of parameter qualifiers. UDT object instances are reference types and always carry the `series` qualifier.

---

## Exporting User-Defined Types (UDTs)

Libraries can export UDTs by prefixing the `type` declaration with the `export` keyword:

```pinescript
//@version=6
library("Geometry")

export type Point
    int x
    float y
    bool isHi
    bool wasBreached = false
```

Another script can import this library and declare instances of `Point`:
```pinescript
//@version=6
indicator("Consumer Script")
import Username/Geometry/1 as geom

// Instantiate the UDT using the namespace alias
geom.Point myPoint = geom.Point.new(bar_index, high, true)
```

> [!NOTE]
> A library UDT only requires `export` if it is passed into, or returned from, an exported function, method, or UDT field. If a UDT is used solely for internal library calculations, it does not need to be exported.

---

## Exporting Enums

Libraries can export enums to share standardized sets of named constants:

```pinescript
//@version=6
library("SignalLib")

//@enum           An enumeration of named signal states.
//@field long     Represents a "Long" signal.
//@field short    Represents a "Short" signal.
//@field neutral  Represents a "Neutral" signal. 
export enum State
    long    = "Long"
    short   = "Short"
    neutral = "Neutral"
```

A script importing this library accesses the enum fields via the namespace alias:
```pinescript
//@version=6
indicator("Strategy Consumer")
import Username/SignalLib/1 as Sig

Sig.State mySignal = switch
    close > ta.highest(high, 20)[1] => Sig.State.long
    close < ta.lowest(low, 20)[1]  => Sig.State.short
    =>                                Sig.State.neutral
```

---

## Importing and Using a Library

To import a library, use the `import` statement:
```pinescript
import <username>/<libraryName>/<libraryVersion> [as <alias>]
```

*   **Version Pinning**: The version number must be explicitly specified (e.g. `/1`). Libraries do not auto-update to ensure dependency stability.
*   **Namespace Alias**: The `as <alias>` part is optional. When specified, all exported functions, methods, UDTs, and enums from that library are accessed through the alias namespace.

```pinescript
//@version=6
indicator("Using AllTimeHighLow library", "", true)
import PineCoders/AllTimeHighLow/1 as allTime

// Reference library exports via the alias
plot(allTime.hi())
plot(allTime.lo())
plot(allTime.hi(close))
```
