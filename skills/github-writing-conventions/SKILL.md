---
name: github-writing-conventions
description: Conventions for GitHub-facing text — issue and PR narration, title casing, cross-repo references, and repo descriptions. TRIGGER - about to run `gh issue create`, `gh pr create`, `gh repo edit --description`, or otherwise write an issue/PR body, a repo's About description, or a comment referencing another repo's issue/PR. SKIP - commit messages (see git-commit-conventions).
---

## Naming vs. Linking References

Backticks name something as an identifier — a repo, an issue/PR, a file — and are fine there whether or not that thing is linkable elsewhere. Whether a reference should also be clickable is a separate choice: match it to whether a click-through is wanted at that point in the text.

An issue or PR link relies on GitHub's autolinking, which fires only on plain text, never inside a code span:

- Same-repo: a bare `#id` in plain text.
- Cross-repo: `org/repo#id` or the full URL, in plain text. Never a bare `#id` cross-repo.
  Why: GitHub links it to whatever issue or PR holds that number in the rendering repo, which is very likely the wrong thing.

Naming an issue/PR rather than linking it — in a possessive phrase, or when it's already linked once in the same body — is fine backticked: `` `threeal/action-starter#1076` ``'s CLAUDE.md implementation replaced...

GitHub never auto-links a bare repo or file string, so naming one produces no link either way. When a repo or file needs to be clickable, write an explicit markdown link:

- Naming a repo: `` `threeal/nodejs-starter` ``'s CLAUDE.md still uses...
- Linking a repo: see [threeal/nodejs-starter](https://github.com/threeal/nodejs-starter)
- Naming a file: `` `src/main.ts` `` handles the entry point
- Linking a file: see [`src/main.ts`](https://github.com/org/repo/blob/main/src/main.ts)

## Issues

Write an issue as narration led by background and rationale — what problem or gap prompted it, what's missing, confusing, or broken, and why that matters. The implementation gets barely any space: a sentence or two near the end, or folded into the closing line.

Prose is the default shape for that narrative, since most issues are one coherent problem description rather than a set of disjoint facts. Use a bullet list, code block, table, or section header wherever it separates genuinely distinct content more clearly than prose would — ordered steps, a short ranking, a before/after comparison, a snippet. The bar is whether the structure earns its place.

## Pull Requests

Always write a meaningful description; never open a PR with an empty or near-empty body.
Why: the point of asking Claude to open the PR is to spare the user writing it themselves.

When the PR closes an issue, open with a closing keyword and reference (e.g. `Closes #44`), then don't restate that issue's background — a brief nod is enough, and the rest of the body goes to the changes actually made.
Why: the keyword both closes the issue on merge and puts the full reasoning one click away.

Don't wrap the whole body in a lone `## Summary` header; write it as plain prose or bullets instead. Add section headers only when the body holds more than one distinct kind of content, such as a summary plus a test plan.

Omit the test plan entirely unless a human has to verify something by hand — skip it when CI or a git hook already covers every relevant check, when the change has no runtime behavior to exercise, or when there's obviously nothing to test. Never include one just to say "N/A".

## Casing and Punctuation

- PR title: capitalize the first letter — `Add login feature`, not `add login feature`.
- Repo description: capitalize the first letter, and end with no period or other punctuation.

A repo description is a single tagline, a sentence fragment rather than a full sentence. Commas can lightly join related qualities (`Fast, minimal HTTP router for Go`), and one `-` or `—` can join a name to a clarifying clause (`Zod — TypeScript-first schema validation`), but don't stack more than one separator. No emoji.
