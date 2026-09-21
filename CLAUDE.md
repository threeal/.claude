This file provides guidance to Claude Code (claude.ai/code) when working with code in any repository.

## Git Workflow

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

When a change needs approval before being written, choose how to present it by how likely it is to be accepted, rather than defaulting to pasting full file content into chat.
Why: reviewing a diff in an editor is far easier than reviewing pasted text.

- Likely to accept, and the file isn't dirty: write it and ask for review. A clean file makes both the diff and a revert easy.
- Likely to object: write nothing yet. Say which files would change, describe the change in general terms rather than as an exact diff, and wait for approval.
- Short enough to read as text: pasting the exact diff or content in chat is fine.

## Maintaining This Project's Own Guidance

This repo is the durable, cross-machine home for guidance that should hold in any repo, unlike a project's local memory, which is per-project and per-machine.
When a memory entry saved in any project reflects general guidance about Claude's behavior, workflow, or formatting conventions — not something specific to that project's own code or context — ask whether it should be proposed as a change here instead of left local to that project.
