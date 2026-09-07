---
name: lefthook-facts
description: Reference facts about Lefthook (lefthook.dev) git hook manager configuration — hook-level and job-level options, what piped/parallel/fail_on_changes actually do. TRIGGER - editing/reviewing a lefthook.yaml (or lefthook.yml/.lefthook.yaml) file, or discussing Lefthook hooks, jobs, or pre-commit tooling built on Lefthook.
---

## Hook-level options (e.g. under `pre-commit:`)

- **`piped: true`** — jobs run **sequentially, in the order listed**, and execution **stops at the first job that fails**. This is a common trap: despite the name, it does **not** pipe one job's stdout into the next job's stdin like a shell pipe. It's purely "run in order, stop on failure."
- **`parallel: true`** — the opposite of `piped`: all jobs run concurrently, independent of each other's outcome. `piped` and `parallel` are mutually exclusive — Lefthook errors if both are set on the same hook.
- **`fail_on_changes`** — controls whether Lefthook exits non-zero when a job **modifies git-tracked files** while running (e.g. a formatter like `dprint fmt` or `prettier --write`). Values: `never` (default — never fail just because files changed), `always`, `ci` (fail only when the `CI` env var is set), `non-ci` (fail only when it's not).
- **`fail_on_changes_diff`** — controls whether the diff of those changes is printed when `fail_on_changes` triggers. Default: print the diff only in CI. Set explicitly to `true`/`false` to force it either way regardless of environment.
- A hook can list jobs under `jobs:` (each with a `name` + `run`/`script`) or the older flat `commands:`/`scripts:` form.

## Job-level options

- **`name`** — label shown in output; also used to merge/override the job across a local config layer.
- **`run`** — an inline shell command for this job (e.g. `run: dprint fmt`, `run: shellcheck canudev.sh`).
- **`script`** — run an external script file instead of an inline command (paired with `runner:` to pick the interpreter).
- **`glob` / `root` / `exclude`** — scope which files the job's command sees.
- **`stage_fixed`** — automatically re-stages files the job modified (pairs naturally with `fail_on_changes: ci` — see the fail_on_changes docs — so local runs auto-fix-and-stage while CI still fails loudly on the same diff). Only works for `pre-commit` hooks.

## Practical pattern seen in projects using this

A `pre-commit` hook with `piped: true` and a `jobs:` list is typically ordered fix-then-lint — e.g. a formatter job (`dprint fmt`, which can modify files, hence `fail_on_changes`/`fail_on_changes_diff` being set) followed by a pure lint job (e.g. `shellcheck <file>`, which never modifies files, so those two settings are simply no-ops for it). Adding a new lint-only job just means appending another `{name, run}` entry to `jobs:` — no need to touch `fail_on_changes`/`fail_on_changes_diff` for it, and no separate CI wiring is needed if CI already invokes `lefthook run pre-commit --all-files` (as opposed to running formatters/linters as their own separate CI steps).

Source: https://lefthook.dev/configuration/
