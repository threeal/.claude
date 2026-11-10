---
name: github-actions-conventions
description: Conventions for writing GitHub Actions workflows, such as where a setup step goes in a job. TRIGGER - about to write or edit a .github/workflows/*.yaml file, or add, move, or reorder a step in a workflow job.
---

When writing or changing a value in the file, also follow `yaml-conventions`.

## Setup Step Placement

Place each setup step right before the step that first uses the tool it sets up, not grouped at the top of the job after checkout.
Why: a setup's position should show what needs it — `Setup Lefthook` goes after `Setup pnpm` because the hook runs jobs that need pnpm, and a `Setup Docker Buildx` needed only by the deploy step goes right before that step, after the registry login.

Checkout has no dependencies, so it comes first by the same rule. A setup that doesn't read the repo, like installing dprint, still goes after checkout, since its position comes from what uses it, not from what it depends on.

When several setups come before the same step, a setup another one depends on comes first, like Java before Gradle. Setups that don't depend on each other follow the order in which the step uses them — for the hook step, the order of its jobs in `lefthook.yaml`. So `Setup Lefthook` comes right before the hook step, after every setup its jobs need.

The rule can give way when following it would cost more than it's worth. The main case is a step that installs several tools at once, like `taiki-e/install-action` installing dprint for the hook and cargo-tarpaulin for `Test`: keep it whole rather than splitting it into separate steps, and place it before the first step that needs any of its tools.
