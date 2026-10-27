---
name: coding-conventions
description: Language-agnostic conventions for writing code, such as deciding what a function, class, or module owns and what to name it. TRIGGER - writing, editing, or refactoring code in any language.
---

## Language-Specific Conventions

Load these alongside this skill when the code matches:

- Node.js code, including a GitHub Action's `src/` or a CLI script: `nodejs-conventions`.
- A JavaScript/TypeScript GitHub Action's source: `javascript-action-conventions`.
- A Vitest test file: `vitest-conventions`.

## Scope

Give each unit (a function, class, module, or anything else with a boundary) a meaningful reason to exist, and have it own only what it does.

Have a unit say only what it knows itself, in its errors, logs, and return values. Leave what a particular caller needs to say to that caller.
Why: a unit shaped around its first caller speaks for that caller, and misleads every other one.

Example: `resolveVersionRange` is called only after a tag lookup fails, and its caller wants one combined message.

- Incorrect: it throws `no tag or version matches "^9"`. It never tried a tag, and any other caller would report one it never tried either.
- Correct: it throws `no version matches "^9"`, and the caller, which knows it tried a tag first, says so in its own message.

## Naming

Name a unit for what it returns, holds, or does now, not for what it was first meant to do.

Example: a function returning a predicate that tells whether a version matches.

- Incorrect: `parseRange`. It doesn't return a parsed range, so every call site reads wrong.
- Correct: `createRangeMatcher`, with its helpers renamed to match.

## Fixing a Wrong Scope or Name

When a unit's scope or name turns out wrong, redraw its boundary or rename it: move the message to the caller, split or merge the unit, or rename it and its helpers. Don't patch around it by rewording a message in place or explaining the name in a comment.
Why: a patch leaves the unit claiming to be something it isn't, so the next caller trips on it again.
