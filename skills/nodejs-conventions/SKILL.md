---
name: nodejs-conventions
description: Conventions for Node.js source code, such as choosing between sync and async APIs. TRIGGER - writing, editing, or refactoring JavaScript/TypeScript code that runs on Node.js, including a GitHub Action's src/ or a CLI script.
---

## I/O

Always call the async version of an I/O API (e.g. `node:fs/promises`), never its sync counterpart (`existsSync`, `readFileSync`, `execSync`), even in an untested entry point like `main.ts`.
Why: concurrent Vitest tests only overlap at `await` points, so sync I/O in tested code quietly makes them run one after another. Code in an untested entry point tends to move into tested modules later, and blocking buys nothing even in a linear script.

To handle a file that may not exist, attempt the operation and handle `ENOENT` instead of checking for the file first; rethrow any other error.
Why: `node:fs/promises` has no `exists`, and attempting directly closes the gap between the check and the use while keeping the missing-file case in the function that does the I/O, where it gets tested.
