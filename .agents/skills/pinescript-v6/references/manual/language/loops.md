# Loops

Loops allow repeating code blocks. They should be used sparingly as they consume runtime resources.

### `for` Loop
```pinescript
//@version=6
indicator("Loops - for")
float totalClose = 0.0
for i = 0 to 9
    totalClose := totalClose + close[i]
plot(totalClose / 10)
```

### `for...in` Loop (Collections)
```pinescript
//@version=6
indicator("Loops - for...in")
float[] arr = array.from(1.0, 2.0, 3.0)
float sum = 0.0
for val in arr
    sum := sum + val
plot(sum)
```

### `while` Loop
```pinescript
//@version=6
indicator("Loops - while")
int counter = 0
while counter < 5
    counter := counter + 1
plot(counter)
```

See also:
- [Arrays](arrays.md)
- [Conditional Structures](conditional-structures.md)
