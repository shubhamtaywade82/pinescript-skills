# Strings

Pine Script® strings are immutable sequences of up to 40,960 characters, including letters, numbers, spaces, and other Unicode symbols. Strings are commonly used for titles, timeframe specifications, alerts, debug messages, and visual text elements.

---

## Defining Literal Strings

### 1. Single-Line Strings
Enclosed in single quotes (`'`) or double quotes (`"`):
```pinescript
string hello1 = "Hello world!"
string hello2 = 'Hello world!'
```
To include quotes inside a single-line string that matches the enclosing delimiter, escape them with a backslash (`\`):
```pinescript
string quotedStr = "This contains \"double quotes\"."
```

> [!WARNING]
> Line wrapping of single-line strings over multiple lines is deprecated in Pine Script v6. Concatenate strings with `+` or use multiline syntax instead.

### 2. Multiline Strings
Enclosed in triple quotes (`"""..."""`) or triple apostrophes (`'''...'''`). Indentations and newlines within the delimiters are preserved literally:
```pinescript
string multilineStr = """
Hello
world!
    Indented line
"""
```

---

## Escape Sequences

Use the backslash (`\`) to define control characters or escape special delimiters:

*   `\\`: Represents a literal backslash.
*   `\'` / `\"`: Represents literal quotes inside matching boundaries.
*   `\n`: Line break (newline).
*   `\t`: Horizontal tab space.

```pinescript
//@version=6
indicator("Control characters demo", overlay = true)

if barstate.islastconfirmedhistory
    string displayText = "First Line\n\tIndented tab line"
    label.new(bar_index, high, displayText, style = label.style_label_left)
```

---

## Concatenation

Use the `+` and `+=` operators to combine strings. Since strings are immutable, each concatenation allocates a new string in memory:

```pinescript
string base = "Pine"
base += " Script" // base is now "Pine Script"
```

---

## Conversion and Formatting

### 1. Converting to Strings (`str.tostring()`)
Converts `int`, `float`, `bool`, arrays, matrices, or enums to strings.
```pinescript
str.tostring(value)
str.tostring(value, format)
```
The `format` parameter supports presets (`format.mintick`, `format.percent`, `format.volume`) or custom pattern strings (using `#` for optional digits, `0` for forced digits, `,` for thousands separators, and `%` for percentages):

```pinescript
//@version=6
indicator("String Conversion Demo")

if barstate.islast
    string vwapCustom = str.tostring(ta.vwap, "#.###")    // E.g. "123.456"
    string vwapFixed  = str.tostring(ta.vwap, "0.00000")  // E.g. "123.45600"
    string volShort   = str.tostring(volume, format.volume) // E.g. "1.2M"
```

### 2. Formatting Strings (`str.format()`)
Injects multiple variables into a formatted string template using `{0}`, `{1}`, etc., placeholders:
```pinescript
string msg = str.format("Close: {0,number,#.##}, Open: {1,number,#.##}", close, open)
```
*   *Note*: Single quotes (`'`) act as quote characters inside `str.format()` to output literal brackets (e.g. `'{'` and `'}'`). Use double single quotes `''` to print a literal `'`.

---

## Modifying Strings

*   **`str.replace(source, target, replacement, occurrence)`**: Replaces the N-th occurrence (0-indexed) of a substring.
*   **`str.replace_all(source, target, replacement)`**: Replaces all instances of a substring.
*   **`str.upper(source)`** / **`str.lower(source)`**: Swaps case for ASCII characters.
*   **`str.trim(source)`**: Strips leading and trailing whitespaces, newlines, and tabs.
*   **`str.repeat(source, repeat, separator)`**: Repeats a sequence N times.

---

## Inspection and Extraction

*   **`str.length(string)`**: Returns the total characters in a string.
*   **`str.contains(source, str)`** / **`str.startswith()`** / **`str.endswith()`**: Returns booleans checking substring patterns.
*   **`str.split(string, separator)`**: Splits a string into an array of substrings.
*   **`str.pos(source, str)`**: Returns the 0-indexed position of the first occurrence of a substring.
*   **`str.substring(source, begin_pos, end_pos)`**: Extracts characters from `begin_pos` (inclusive) to `end_pos` (exclusive).

---

## Matching Patterns with Regular Expressions (Regex)

The `str.match(source, regex)` function returns the first matched sequence or an empty string `""` if no match is found. Regex backslashes must be escaped twice in string declarations (e.g. use `\\w` for word characters).

```pinescript
//@version=6
indicator("Matching substrings with regex demo")

string symbolInput = input.symbol("NASDAQ:AAPL", "Symbol")

// Matches BATS:, NASDAQ:, NYSE:, or AMEX: at the start
var bool supportedSymbol = str.match(symbolInput, "^(?:BATS|NASDAQ|NYSE|AMEX):") != ""

if supportedSymbol
    // Lookbehind check for colon, capturing everything after it
    var string noPrefix = str.match(symbolInput, "(?<=:).+")
    log.info("Symbol without prefix: {0}", noPrefix)
```

### Supported Regex Syntax Highlights

*   **Predefined Classes**: `.` (any char), `\\d` (digit), `\\w` (word char), `\\s` (whitespace).
*   **Quantifiers**: `?` (optional), `*` (0 or more), `+` (1 or more), `{n,m}` (n to m times).
*   **Logical**: `|` (OR), `[...]` (character class), `[^...]` (negated class).
*   **Assertions**: `^` / `\\A` (start of line/string), `$` / `\\Z` (end of line/string), `(?<=...)` (positive lookbehind), `(?=...)` (positive lookahead).
*   **Modifiers**: `(?i)` (case-insensitive), `(?m)` (multiline mode), `(?s)` (single-line dotall mode).
