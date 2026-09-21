---
name: yaml-conventions
description: Conventions for writing YAML itself, independent of which tool consumes the file. TRIGGER - about to write or edit any YAML file (.yaml/.yml), or add or change a value in one. SKIP - what a particular tool's YAML keys mean (see that tool's own skill).
---

## Scalar Quoting

Default to plain, unquoted scalars. Quote only where YAML requires it — the plain form would be a syntax error, or would resolve as a non-string type.
Why: reflexive quoting spends the readability plain scalars exist to give, and once a file mixes required quotes with habitual ones, the quotes stop marking anything.

Decide per scalar, not per list or per file:

```yaml
patterns:
  - vitest
  - "@vitest/coverage-v8"
```

When the consuming parser's YAML version is unknown, assume 1.1 — quote `yes`/`no`/`on`/`off` and bare dates too.

Don't restyle quotes on lines you aren't otherwise changing.
