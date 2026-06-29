# Welcome to Pine Script® v6

Welcome to the Pine Script® v6 User Manual. Pine Script® is TradingView's programming language designed to create custom indicators, strategies, and libraries to run on TradingView charts.

This manual provides an in-depth guide to using Pine Script® v6 to build advanced trading analysis tools.

### Manual Structure

- **[Pine Script® Primer](primer/index.md)**: Getting started, setting up the Pine Editor, and creating your first indicator.
- **[Language](language/index.md)**: Core language mechanics, execution model, type system, custom objects, enums, methods, and collections.
- **[Visuals](visuals/index.md)**: Plotting data, coloring bars, adding labels, lines, boxes, and creating complex tables.
- **[Concepts](concepts/index.md)**: Advanced script behavior including alerts, barstates, inputs, libraries, timeframes, and strategies.
- **[Writing Scripts](writing-scripts/index.md)**: Best practices, styling guidelines, debugging techniques, profiling, and publishing scripts.
- **[Errors and Warnings](errors/index.md)**: Compilation and runtime error reference code maps.

---

### Key v6 Highlights
- Strict boolean rules (no implicit numeric/conditional coercion).
- Object history references must be parenthesized: `(obj[1]).field`.
- Built-in enum types and custom dropdowns using `input.enum()`.
- Detailed Pine Logs console debugging using `log.info()`, `log.warning()`, and `log.error()`.
