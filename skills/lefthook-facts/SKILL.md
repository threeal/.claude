---
name: lefthook-facts
description: Reference facts about Lefthook (lefthook.dev) git hook manager configuration — hook-level and job-level options, and what piped/parallel/fail_on_changes actually do. TRIGGER - editing/reviewing a lefthook.yaml (or lefthook.yml/.lefthook.yaml) file, or discussing Lefthook hooks, jobs, or pre-commit tooling built on Lefthook.
---

When writing or changing a value in the file, also follow `yaml-conventions`.

## Hook-Level Options

- `piped: true` — jobs run sequentially in listed order, stopping at the first failure. Despite the name it does **not** pipe one job's stdout into the next job's stdin.
- `parallel: true` — all jobs run concurrently, independent of each other's outcome. Mutually exclusive with `piped`; setting both is an error.
- `fail_on_changes` — whether Lefthook exits non-zero when a job modifies git-tracked files (e.g. `dprint fmt`). Values: `never` (default), `always`, `ci` (only when the `CI` env var is set), `non-ci`.
- `fail_on_changes_diff` — whether the diff of those changes is printed. Defaults to printing only in CI; `true`/`false` forces it either way.
- Jobs go under `jobs:`, each with `name` plus `run`/`script`, or in the older flat `commands:`/`scripts:` form.

## Job-Level Options

- `name` — label shown in output, and the key used to merge or override the job from a local config layer.
- `run` — an inline shell command.
- `script` — an external script file instead, paired with `runner:` to pick the interpreter.
- `glob` / `root` / `exclude` — scope which files the job's command sees.
- `stage_fixed` — re-stages files the job modified. Works only for `pre-commit` hooks, and pairs naturally with `fail_on_changes: ci`, so local runs auto-fix and stage while CI still fails on the same diff.

Source: https://lefthook.dev/configuration/
