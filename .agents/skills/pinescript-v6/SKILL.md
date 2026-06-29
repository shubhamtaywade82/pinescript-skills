---
name: pinescript-v6
description: Guide and rules for writing, refactoring, and debugging Pine Script v6 scripts (TradingView indicator, strategy, and library). Use when the user asks to write, edit, migrate, or debug Pine Script code.
---
# Pine Script® v6 Developer Skill

This skill provides guidelines, style rules, and reference structures for writing high-quality TradingView Pine Script® v6 code. Use this skill to ensure compliance with v6 compiler rules, types, methods, and practices.

## Core Rules for Pine Script v6

1. **Compiler Directive**: Every script must start with `//@version=6` (unless a different version is explicitly requested).
2. **Script Structure**:
   - Scripts follow a strict three-part order: `version`, declaration statement, then code.
   - Use exactly one declaration statement: `indicator()`, `strategy()`, or `library()`.
   - Indicators must call at least one output function such as `plot()`, `plotshape()`, `barcolor()`, `line.new()`, `log.info()`, or `alert()`.
   - Strategies must call at least one order placement command or other output function.
   - Libraries must export at least one user-defined function, method, type, or enum.
```pinescript
//@version=6
indicator("My Script")
plot(close)
```
3. **Identifiers**:
   - Start with a letter (`A-Z` or `a-z`) or underscore (`_`).
   - Follow with letters, underscores, or digits.
   - Pine Script is case-sensitive.
   - Style guide: use uppercase `SNAKE_CASE` for constants and `camelCase` for other identifiers.
4. **Stricter Booleans**:
   - `bool` values can NEVER be `na`.
   - No implicit casting from `int`/`float` to `bool`. Use explicit comparisons: `if volume != 0` instead of `if volume`.
   - Do NOT use `na()`, `nz()`, or `fixnan()` with boolean values.
5. **Control Flow**:
   - The `when` parameter is removed from all `strategy.*` functions. Use `if` statements instead.
```pinescript
if buy_condition
    strategy.entry("Long", strategy.long)
```
6. **History Reference on Objects**:
   - Do not write `myObject.field[1]`.
   - Use `(myObject[1]).field` or assign the field to a local variable first.
```pinescript
temp = myObject.field
val = temp[1]
```
7. **Enums & Dropdowns**:
   - Prefer `enum` with `input.enum()` for type-safe settings dropdowns.
   - Enum members map human-readable labels by string values.
8. **Dynamic Requests**:
   - `request.security()`, `request.financial()`, `request.dividends()`, and related functions can be called inside loops, `if`/`switch` blocks, and user-defined functions. They accept series values for symbols and timeframes.
   - Each unique combination of symbol, timeframe, and expression counts as one request, and scripts are limited to 40 unique requests.
9. **Logging for Debugging**:
   - Use `log.info()`, `log.warning()`, and `log.error()` with `{0}` placeholder formatting.
   - Do not rely on legacy plot-based debug hacks for control flow.

## Reference Guides (Read when needed)
Refer to these markdown files in the `references/` directory for detailed explanations:
- [Type System Rules](file:///home/nemesis/project/trading-workspace/pinescript/pinescript-skills/.agents/skills/pinescript-v6/references/type-system.md): Strict boolean rules, lazy evaluation, and types.
- [Collections & UDTs](file:///home/nemesis/project/trading-workspace/pinescript/pinescript-skills/.agents/skills/pinescript-v6/references/collections.md): Custom types, methods, arrays, maps, matrices, and history referencing.
- [Dynamic Requests](file:///home/nemesis/project/trading-workspace/pinescript/pinescript-skills/.agents/skills/pinescript-v6/references/dynamic-requests.md): Dynamic `request.security` and loops over arrays of symbols.
- [Strategies & Visuals](file:///home/nemesis/project/trading-workspace/pinescript/pinescript-skills/.agents/skills/pinescript-v6/references/strategies-visuals.md): Strategy execution, margin, polylines, logging, inputs (active, inline).
- [Execution Model](file:///home/nemesis/project/trading-workspace/pinescript/pinescript-skills/.agents/skills/pinescript-v6/references/execution-model.md): Execution flow, barstate variables, historical vs. realtime bars.
- [Migration Guide (v5 to v6)](file:///home/nemesis/project/trading-workspace/pinescript/pinescript-skills/.agents/skills/pinescript-v6/references/migration-guide.md): Step-by-step conversion instructions.
- [Reference Manual Extracts](file:///home/nemesis/project/trading-workspace/pinescript/pinescript-skills/.agents/skills/pinescript-v6/references/reference-extracts.md): Extracted type, variable, constant, function, keyword, operator, and annotation names from the v6 Reference manual.
