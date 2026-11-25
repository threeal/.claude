---
name: starter-readme-conventions
description: What a starter template's README should and shouldn't explain — assumed reader knowledge, what tooling already catches, opt-outs, and the Customizing section. TRIGGER - about to write, edit, or trim the README.md of `threeal/project-starter` or a `*-starter` repo derived from it, or review what one covers.
---

This is on top of `prose-conventions`'s general rules for writing and editing prose; what follows is about what to include, not how to word it.

## Leave Out What the Reader Already Has

Assume the reader knows the stack. Don't explain the language's or build tool's standard conventions — the standard source layout, how to move a package, or that removing a plugin also means removing its config and version entries.

Don't document what a cheap build or test run catches, as long as its error points to the fix — a stale package name that fails the compile or a coverage check needs no rename steps.

Do document it when the error doesn't point to the fix, especially when the cause is a choice the repo made rather than the tool's default.
Why: the reader can't recover from an error that never names the fix, and can't guess a non-default setup from knowing the tool. Example: Gradle's lock-state failure never mentions `./gradlew dependencies --write-locks`, and dependency locking isn't a Gradle default, so that note stays.

## Describe Opt-Outs as Opt-Outs

An exclusion list — coverage, lint, or similar — is for what the user chooses to leave out. Say what it's for, never tell users to add each new file or package to it.
Why: instructing additions by default steers users away from checking code they might want covered.

## The Customizing Section

Keep it to the parts that change: one short item per file or directory to replace, plus one sentence for optional removals (like an example CLI). No checklist of every line that names a placeholder.
