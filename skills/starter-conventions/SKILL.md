---
name: starter-conventions
description: What a starter template is for, and how that goal judges its tooling, structure, config, and defaults. TRIGGER - about to choose, add, remove, or evaluate tooling, project structure, config, supported platforms, or defaults in `threeal/project-starter` or a `*-starter` repo derived from it, or to review or plan such a choice.
---

## What a Starter Is For

A starter is the foundation for real projects, small or large. Its example code — a Fibonacci CLI, a click counter — is only a placeholder. A project made from it should only need to replace the example with its real app and change almost nothing else.

Judge every tooling, structure, or config choice against that goal, never against the size of the placeholder app.
Why: a placeholder is small by design, so judging by it rejects what a real project needs. A project generator like XcodeGen or Tuist looks like more than "a single-screen app needs", yet it's what keeps an Xcode project's config readable and editable without the GUI as the project grows.

## Minimal but Ready

Keep the config minimal — no bloat, no unused lines — but ready for a real project without more setup.

A tool that a real project would otherwise have to add later belongs in the starter. A line that does nothing for the example or for a real project doesn't.

## Plain Defaults, Variations in the README

Default to the plainest setup. Leave optional variations — removing the example CLI, adding another platform — out of the template and explain them in the README instead.
Example: a SwiftUI starter defaults to macOS only, with a README note on adding iOS, rather than supporting both out of the box.

## Command-Line First

Every workflow — build, test, lint, format, run — must work smoothly from the command line, even when the stack has a GUI such as Xcode.
