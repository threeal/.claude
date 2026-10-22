---
name: javascript-action-conventions
description: How to structure and log a JavaScript GitHub Action's source code — what stays in main.ts, when to extract a function, how logs should read, and how quietly to run commands. TRIGGER - writing, editing, or refactoring the source of a JavaScript/TypeScript GitHub Action (an action.yml with `runs.using: node*`, e.g. its src/main.ts).
---

These rules assume a linear flow: one entry point that runs its steps in order. An action with a `post` step or several separate phases gets one such script per entry point.

Before writing, ask the user which existing action to follow as a worked example, unless they already named one.
Why: the best example changes as actions get refactored, so a fixed one goes stale.

## Structure

Write the whole flow in `main.ts` as one readable script, as if nothing were split out yet. When refactoring, rewrite the flow there fresh instead of patching existing functions into place.

Pull a part out into its own function only when it earns one:

- It can fail: every failure path lives in a function with its own tests. Success paths stay in `main.ts`, covered by running the action in CI.
- It repeats: e.g. a `run` helper that logs and runs a command, used by several steps.

Everything else stays in `main.ts`, including thin command wrappers and trivial one-line throws. Inlining a function back is as valid as pulling one out.

Extracted functions don't log. They return a value or throw, and `main.ts` decides what to say.
Why: the flow and its logs then read in one place.

## Logs

Write the log as a story of the flow for someone who knows nothing about the action:

- Announce each step before it runs, so any error that follows has context.
- Give each decision its own line, e.g. which version was chosen, or that a cached copy is used.
- After a caught error, log it, then say what happens instead, e.g. a fallback.

To check this, go through every input case, normal and edge, and read the resulting flow and log as that user would. Trace each case through `main.ts`; run the built action locally per case only when asked.

## Command Output

Run commands quietly, and drop log groups that existed only to hide their output.
Why: verbose output is almost never useful — what matters is which step ran and the error when it fails.

- A quiet flag must not also silence errors — e.g. `curl -s` hides the error message, so pair it with `-S`.
- A command that runs for minutes still needs some progress output, or the job looks stuck.
