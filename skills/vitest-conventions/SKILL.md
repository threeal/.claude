---
name: vitest-conventions
description: Conventions for structuring and configuring Vitest test files — concurrency, describe grouping, and the options form for describe/test. TRIGGER - about to write or edit a Vitest test file.
---

## Concurrency

Default to `concurrent: true` for suites and tests, especially async ones. Non-concurrent should be the exception — shared mutable state, an ordering dependency — not the default.

## Grouping

Group a function's unit tests under a `describe` named after the function, even when the file currently covers only one function.
Why: keeps the shape consistent as more functions get added later, and test output reads as "function → its cases" from day one.

## Options Form

Prefer the object-options form over chained modifiers, for both `describe` and `test`:

- Correct: `describe(name, { concurrent: true }, fn)`
- Incorrect: `describe.concurrent(name, fn)`

Why: options are easy to extend later (e.g. adding `timeout` or `retry`) without changing the call shape.
