# Language

Pine Script® v6 is a domain-specific language designed specifically for financial charts and calculations. It is a statically-typed language with dynamic time series support.

### Contents

- **[Execution Model](execution-model.md)**: How scripts run bar-by-bar, ticks, and rollback.
- **[Type System](type-system.md)**: Types (int, float, bool, color, string) and type qualifiers (const, input, simple, series).
- **[Script Structure](script-structure.md)**: Annotations, declaration statement, variable assignments, and outputs.
- **[Identifiers](identifiers.md)**: Variable and function naming rules.
- **[Declaration Statements](declaration-statements.md)**: `indicator()`, `strategy()`, and `library()`.
- **[Variable Declarations](variable-declarations.md)**: Variable definitions, reassignments (`:=`), and persistence (`var`, `varip`).
- **[Operators](operators.md)**: Mathematical, comparison, logical, history referencing `[]`, and ternary.
- **[Conditional Structures](conditional-structures.md)**: Branching logic using `if` and `switch` expressions.
- **[Loops](loops.md)**: Iteration using `for`, `for...in`, and `while`.
- **[Built-ins](built-ins.md)**: Accessing built-in functions, constants, and variables.
- **[User-Defined Functions](user-defined-functions.md)**: Custom function creation.
- **[Objects (UDTs)](objects.md)**: Defining and using custom User-Defined Types (UDTs).
- **[Enums](enums.md)**: Declaring enums and creating settings dropdowns via `input.enum()`.
- **[Methods](methods.md)**: Defining custom methods for dot-notation calling.
- **[Arrays](arrays.md)**: Storing values in dynamically-sized arrays.
- **[Matrices](matrices.md)**: Two-dimensional data structures.
- **[Maps](maps.md)**: Key-value associative collections.
