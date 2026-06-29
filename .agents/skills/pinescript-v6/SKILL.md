---
name: pinescript-v6
description: Use this skill for Pine Script v6 authoring, review, and troubleshooting. Handles v6 API, compiler rules, UDTs, request.* semantics, logging, annotations, type qualifiers, migration from v5, debugging compiler/runtime errors, and creating complete reference guidance. Use when the user edits, writes, reviews, or debugs Pine Script v6 code.
metadata:
  brand: pine-script
  version: "6"
---

# Pine Script® v6

This skill defines the core rules, structure, and reference pointers for creating, editing, migrating, and debugging Pine Script v6 scripts while maintaining the project documentation standards below.

## Core rules

1. Every script must start with `//@version=6`.
2. Scripts must contain exactly one declaration statement: `indicator(...)`, `strategy(...)`, or `library(...)`.
3. Indicators must call at least one output function such as `plot(...)`, `plotshape(...)`, `barcolor(...)`, `line.new(...)`, `log.info(...)`, or `alert(...)`.
4. Strategies must call at least one order placement or strategy output command.
5. Libraries must export at least one exported function, method, type, or enum.
6. Always prefer explicit typing over implicit inference in public API and migration examples.
7. Keep bool checks explicit in v6: no implicit numeric coercion. Use `if volume != 0` instead of `if volume`.
8. History referencing on object fields must use `(myObject[1]).field` or a local variable instead of `myObject.field[1]`.
9. Default to verbose but structured status lines when debugging. Use `log.info(...)`, `log.warning(...)`, and `log.error(...)` with placeholders. Do not leave plot-based debugging hacks.
10. Treat any deviations flagged by the compiler as permanent prefs unless the user explicitly asks to override.

## Style guidelines

- Use uppercase SNAKE_CASE for constants.
- Use camelCase for variables, functions, and types.
- Use enum with `input.enum(...)` for dropdown settings.
- Do not use legacy Pine v4 style history operators when migrating to v6 unless the user specifically asks for it.

## Authoring and migration guidelines

- Use v6 APIs and signatures. Use request.* security/financial/dividends APIs with series values where applicable.
- Keep changes minimal and reversible.
- Do not refactor surrounding code unless required by a v6 migration or compiler rule.
- Keep examples short, local to the changed file, and idiomatic for Pine.

## Reference guides (read when needed)

Use the bundled references in `references/` as the live guide for this project.

### User Manual map

- [User manual landing](manual/index.md): Welcome to Pine Script® v6.
- [Primer / First steps](manual/primer/first-steps/index.md): Getting started with Pine and your first indicator.
- [Language](manual/language/index.md): Execution model, type system, custom objects, enums, methods, collections (arrays, matrices, maps), expressions, loops, built-ins, and UDFs.
- [Visuals](manual/visuals/index.md): Plots, tables, labels, lines, boxes, fills, colors, and shapes.
- [Concepts](manual/concepts/index.md): Alerts, sessions, timeframes, repainting, libraries, and chart data.
- [Writing scripts](manual/writing-scripts/index.md): Style guide, debugging, profiling, publishing, and limitations.
- [Errors and warnings](manual/errors/index.md): CE10101, CW10003, RE10139, RE10143, and the current documented codes.

### Bundled reference docs

- [v6 Reference Guide](references/v6-reference/index.md): Types, constants, functions, operators, keywords, and annotations.
- [Reference Extracts](references/reference-extracts.md): Alphabetical reference summary.
- [Reference HTML](references/reference.html): Raw manual extract used for offline lookup.
- [Built-ins summary](references/builtins-reference.md): Concise built-in reference.
- [Full docs copies](references/docs-references.md): Full docs as copied per current session state.
- [Type System Rules](references/type-system.md): Type qualifiers and strict boolean rules.
- [Execution Model](references/execution-model.md): Historical buffers, barstate, and lazy evaluation.
- [Collections & UDTs](references/collections.md): Custom types, arrays, maps, matrices, and methods.
- [Dynamic Requests](references/dynamic-requests.md): `request.*` inside loops, `if` blocks, and user-defined functions.
- [Strategies & Visuals](references/strategies-visuals.md): Strategy execution, order placement, plots, tables, and inputs.
- [Migration Guide v5 -> v6](references/migration-guide.md): Step-by-step migration notes.
- [Errors overview](references/errors.md): Overview of runtime errors, compilation errors, and compiler warnings.

These docs are kept current alongside the official Pine Script v6 documentation.
