This file provides guidance to Claude Code (claude.ai/code) when working with code in any repository.

## Git Workflow

This section covers whether you're allowed to commit, push, stage, or merge — not how to write or sign the commit itself; see `git-commit-conventions` for that.

Never run `git commit` or `git push` unless explicitly told to.
Why: every change is reviewed by the user before it's committed.

Never run `git add` when asked to commit.
Why: the user may have already staged a specific subset of files, and whatever is left unstaged at commit time is intentional.

Never modify any file's staging status (`git add`, `git restore --staged`, `git reset`) unless explicitly allowed or asked.
Why: staged files mark a deliberate checkpoint of what the user has reviewed and agreed to.
If a task seems to require a staging change, that's a sign the task or approach is wrong — stop and flag it instead of touching staging to make it work.

Creating a PR is fine, but never merge one — not `gh pr merge`, not any API call that merges — even when explicitly told to.
Why: merging is how the user signals acceptance, and that signal has to be their own button click, even on their own PRs.
If asked to merge, ask for confirmation first and merge only after an explicit yes.

## Implementation Approval

Never start writing or editing code until explicitly told to proceed ("go ahead", "implement it", "do it").
Why: discussing a design, describing a plan, or answering one clarifying question — including via AskUserQuestion — resolves only that point, not the whole discussion. Auto mode changes how much to ask before acting, not this.
If it's unclear whether the user is ready for implementation, ask before touching any files.

## Implementation Scope

When asked to implement or modify code, only write the code. Don't run anything afterward to check, verify, or act on the result unless explicitly asked — not tests, linters, type checkers, builds, formatters, dependency installs after a manifest change, a script just written, a migration, or anything else that executes the code or acts on its effects.
Why: assume the code is correct on the first attempt instead of relying on a write-then-check loop. When it's wrong, the user will say what to write instead.
Auto mode licenses only read-only, state-free commands — never one run as a side effect of an implementation task, however routine it normally is.

## Implementation Changes

When a fix requires changing the underlying implementation — a different approach, a new abstraction, removing an existing feature — describe the change and its trade-offs first, then wait for explicit approval before editing.
Why: explaining a root cause is not permission to implement a different solution.

## Proposing File Changes for Review

When a change needs approval before being written, decide venue first, then detail: how likely the change is to be accepted decides whether it lands in the file or in chat; size decides how much detail chat gets.
Why: reviewing a diff in an editor is far easier than reviewing pasted text, and predicted acceptance is independent of size — a short change can still be one to describe rather than write.

- Likely to be accepted: write it to the file and ask for review. This is the proposal itself, not a violation of Implementation Approval above — regardless of whether the file was already dirty.
- Likely to draw an objection: write nothing yet. Put it in chat instead — the exact diff when it's short enough to read as text, otherwise which files would change plus a general description — then wait for approval.

## Maintaining This Project's Own Guidance

This repo is the durable, cross-machine home for guidance that should hold in any repo, unlike a project's local memory, which is per-project and per-machine.
When a memory entry saved in any project reflects general guidance about Claude's behavior, workflow, or formatting conventions — not something specific to that project's own code or context — ask whether it should be proposed as a change here instead of left local to that project.
