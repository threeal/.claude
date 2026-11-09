---
name: dependency-conventions
description: Conventions for choosing the version of a dependency or tool, in any ecosystem. TRIGGER - about to add or change a versioned dependency or tool anywhere — a manifest dependency, the `packageManager` field, a GitHub Action tag in a workflow, a toolchain version, or similar.
---

## Use the Latest Version

Always use the latest stable version of a dependency whenever possible, and raise the declared minimum to it — e.g. `^1.1.2`, not `^1.1.0`, once 1.1.2 is out.
Why: a declared minimum is a compatibility promise, and CI only ever tests the version in the lockfile, which is the latest. A lower floor promises support for versions nothing checks — backward-compatibility work taken on before anyone needs it. Raising the floor only drops versions below it, and package managers already resolve to the newest compatible version, so the cost to a consumer shows up only when one is genuinely stuck on an older version. Dependabot's `versioning-strategy: increase` follows the same reasoning: raising the minimum changes nothing about what `^` allows above it, but rejects anything older than the latest outright. Cargo doesn't accept `increase`, only `auto` and `lockfile-only`, and its default raises a requirement only when the new version falls outside it, so Dependabot moves only the lockfile and the floor has to be raised by hand.

Write the full version even where a shorter form is accepted: `"4.6.7"`, as `cargo add` writes it, not `"4"`.
Why: a shortened requirement looks current but declares a floor at its omitted zeros — Cargo reads `"4"` as 4.0.0 and `"1.1"` as 1.1.0.

Look up the latest version from its source — the package registry, the Action's releases, the toolchain's release page — at the time of the change. Never copy it from another repo, an older lockfile, or memory.
Why: Dependabot opens and merges its PRs per repo on its own schedule, so a version found in another repo, even a lineage parent, only says when that repo's last bump was merged, not what's current or agreed on.

This applies to anything with a version, not just npm packages: the `packageManager` field, GitHub Action tags, toolchain versions, and so on. Dependabot keeps manifest dependencies and Action tags current, but nothing bumps `packageManager` automatically, so update it by hand whenever a change touches it.

"Whenever possible" leaves room for real constraints:

- A release still inside pnpm's `minimumReleaseAge` window or Dependabot's cooldown isn't available yet.
- A peer dependency or an incompatible change can hold a package back.
- An action that installs tools from manifests bundled with its own releases, like `taiki-e/install-action`, follows its major tag (`@v2`) instead of an exact version. Its releases are mostly manifest updates for unrelated tools, so an exact pin freezes the installed tools and turns each of those releases into a Dependabot PR, while the major tag keeps the installed tools themselves at their latest version.

Lower a floor only when a real consumer — another project, or someone else — is blocked by it, and then keep it there deliberately. Outside that and the constraints above, treat the latest-version rule as absolute.
