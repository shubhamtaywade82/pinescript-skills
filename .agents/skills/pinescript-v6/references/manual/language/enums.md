# Enums

Enums (enumerations) represent a collection of named constant members. They are ideal for script settings dropdowns.

### Declaring and Using Enums
```pinescript
//@version=6
indicator("Enum Demo", overlay=true)

// Declare the Enum
enum MAType
    SMA
    EMA
    WMA

// Create setting dropdown
maSelect = input.enum(MAType.SMA, "Moving Average Type")

float ma = switch maSelect
    MAType.SMA => ta.sma(close, 20)
    MAType.EMA => ta.ema(close, 20)
    MAType.WMA => ta.wma(close, 20)
    => na

plot(ma, color=color.orange)
```

See also:
- [Inputs](../../concepts/inputs.md)
