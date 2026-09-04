This file provides guidance to Claude Code (claude.ai/code) when working with code in any repository.

## Git Workflow

Never run `git commit` or `git push` unless explicitly told to do so. All changes must be reviewed before being committed.

Never run `git add` when asked to commit — the user may have already staged specific files, and unstaged changes during a commit are intentional.

Never modify the staging status of any file (`git add`, `git restore --staged`, `git reset`, etc.) unless the user explicitly allows or asks for it. The user routinely stages a subset of files before a commit as a deliberate checkpoint marking which changes they've reviewed and agreed to. If a task seems to require changing what's staged or unstaged, stop and flag it instead — that's a sign something is wrong with the task or approach, not a cue to touch the staging area.

Creating a PR is fine, but never merge one (`gh pr merge`, clicking merge via API, etc.) even if the user explicitly says to merge it. Merging requires human approval via the actual button click — that's how the user signals acceptance, even for their own PRs. If asked to merge, ask for confirmation first, and only merge after the user explicitly answers yes to that confirmation.

## Implementation Approval

Never start writing or editing code until I explicitly say to proceed (e.g. "go ahead," "implement it," "do it"). Discussing a design, brainstorming, describing a plan, or my answering a clarifying question (including via AskUserQuestion) is not a greenlight — it only resolves that specific point, not the whole discussion. Never start implementing based on your own judgment of what seems like the right next step; only implement when I explicitly say so. This also applies in auto mode: auto mode licenses working through a task without stopping to ask clarifying questions, but it does not license starting implementation on your own initiative — the explicit go-ahead requirement still applies. If it's unclear whether I'm ready for implementation, ask before touching any files.

## Implementation Scope

When asked to implement or modify code, only write the code. Do not run anything afterward to check, verify, or act on the result of what you wrote — not even to verify an edit you're personally unsure about. This is not limited to checks, tests, linters, type checkers, builds, or formatters — it also covers installing or updating dependencies after changing a manifest/lockfile, running a script you just wrote or edited, applying a migration, or any other command that executes the code or acts on its effects. This applies even in projects with many configured checks. Only run such a command when the user explicitly asks for it. Assume the code you write is correct on the first attempt rather than relying on a write-then-check loop; if it turns out wrong, the user will add guidance on what to write instead, rather than have you catch it via checks.

This also governs auto mode: auto mode only licenses running commands that are read-only and state-free — commands that gather information without changing the state of any file, including committed code, data, or config. It never licenses running a command as a side effect of an implementation task, no matter how routine that command normally is.

## Implementation Changes

When a fix requires changing the underlying implementation (e.g. replacing one approach with a different one, adding new abstractions, removing existing features), always describe the proposed change and the trade-offs first, then wait for explicit approval before making any edits. Do not conflate "explaining the root cause" with permission to implement a different solution.
