---
name: vitest-conventions
description: Conventions for structuring and configuring Vitest test files — concurrency, describe grouping, and the options form for describe/test. TRIGGER - about to write or edit a Vitest test file.
---

## Concurrency

Default to `concurrent: true` for suites and tests that contain async work. Non-concurrent should be the exception there — shared mutable state, an ordering dependency — not the default.

Under Vitest browser mode, leave the option off for tests that render into the DOM; tests that don't touch the page keep the default above.
Why: concurrent tests there run at the same time in the same iframe, sharing one live `document`, so one test's queries can see or miss another's render. The collision surfaces as unrelated-looking timeouts, not an obvious error.

Leave the option off when every test in scope is synchronous.
Why: Vitest runs a concurrent group with `Promise.all`, so tests only overlap at `await` points — a synchronous test runs to completion before the next one starts. The option then has no effect, yet still tells a reader the tests overlap and must not share state.

## Grouping

Group a function's unit tests under a `describe` named after the function, even when the file currently covers only one function.
Why: keeps the shape consistent as more functions get added later, and test output reads as "function → its cases" from day one.

## Options Form

Prefer the object-options form over chained modifiers, for both `describe` and `test`:

- Correct: `describe(name, { concurrent: true }, fn)`
- Incorrect: `describe.concurrent(name, fn)`

Why: options are easy to extend later (e.g. adding `timeout` or `retry`) without changing the call shape.
