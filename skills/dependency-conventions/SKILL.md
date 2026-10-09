---
name: dependency-conventions
description: Conventions for choosing the version of a dependency or tool, in any ecosystem. TRIGGER - about to add or change a versioned dependency or tool anywhere — a manifest dependency, the `packageManager` field, a GitHub Action tag in a workflow, a toolchain version, or similar.
---

## Use the Latest Version

Always use the latest stable version of a dependency whenever possible, and raise the declared minimum to it — e.g. `^1.1.2`, not `^1.1.0`, once 1.1.2 is out.
Why: these projects have no outside consumers to keep working on older versions, and supporting old versions as well costs more than requiring the latest. Dependabot's `versioning-strategy: increase` follows the same reasoning: raising the minimum changes nothing about what `^` allows above it, but rejects anything older than the latest outright.

Look up the latest version from its source — the package registry, the Action's releases, the toolchain's release page — at the time of the change. Never copy it from another repo, an older lockfile, or memory.
Why: Dependabot opens and merges its PRs per repo on its own schedule, so a version found in another repo, even a lineage parent, only says when that repo's last bump was merged, not what's current or agreed on.

This applies to anything with a version, not just npm packages: the `packageManager` field, GitHub Action tags, toolchain versions, and so on. Dependabot keeps manifest dependencies and Action tags current, but nothing bumps `packageManager` automatically, so update it by hand whenever a change touches it.

"Whenever possible" leaves room for real constraints:

- A release still inside pnpm's `minimumReleaseAge` window or Dependabot's cooldown isn't available yet.
- A peer dependency or an incompatible change can hold a package back.

A library with many outside users could one day justify keeping a lower floor on purpose, but no current project is like that, so treat the latest-version rule as absolute outside the constraints above.
