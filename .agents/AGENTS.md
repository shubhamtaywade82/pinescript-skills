# Project Rules for Pine Script® v6 Development

When working in this project, you must adhere strictly to the following rules:

1. **Header**: Always include `//@version=6` at the beginning of all Pine Script files.
2. **Boolean Checks**: Never write implicit boolean checks on numbers. Use explicit comparisons (e.g., `x != 0`, `not na(y)`).
3. **No `na` on Booleans**: Do not call `na(boolValue)`, `nz(boolValue)`, or set `boolValue = na`.
4. **Strategy Order Entries**: Remove `when` parameters from strategy actions; use `if` blocks instead.
5. **UDT Field History**: Access UDT field history via `(object[1]).field` or local variables.
6. **Logging**: Use `log.info()`, `log.warning()`, or `log.error()` for debugging outputs instead of legacy plot hacks.
7. **Inputs**: Prioritize `input.enum()` over `input.string()` for multiple-choice lists. Use `active` and `inline` parameters for clean user interfaces.
