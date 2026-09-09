---
name: markdown-conventions
description: Prefer plain lists over tables, and one line per paragraph/list item over hard-wrapping, when writing markdown. TRIGGER - about to write or edit markdown content — a .md file, a GitHub issue/PR/commit body, or a markdown code comment. SKIP - when the repo's configured formatter already enforces a specific table/wrap convention of its own; defer to that instead.
---

## Prefer Lists Over Tables

Prefer lists over tables, unless the content is genuinely multi-dimensional. A two-column "name → file" mapping is a list, not a table. Tables cost column-width upkeep on every edit (realigning `---|---` dividers and padding) for no reader benefit over a list — not for a human, and not for a model, which doesn't need visual column alignment to parse the association. Reach for a table only when there's a real comparison across several columns.

## Prefer One Line Per Paragraph or List Item

Prefer one line per paragraph or list item over hard-wrapping to a column width. A wrapped paragraph reads as identical prose either way, so wrapping buys little. What it costs: a small edit near the start of a paragraph can cascade into a multi-line diff instead of a one-line diff, which is pure noise on every future edit. Most real reading paths (a rendered view on a hosting platform, an editor with soft-wrap) don't need hard-wrapping anyway — the benefit is confined to viewing contexts that don't soft-wrap, which are a minority.

## Check for a Configured Formatter First

Check whether a configured formatter would enforce or fight a convention before choosing it. For example, Prettier's `proseWrap` option defaults to `"preserve"`, and `dprint-plugin-markdown`'s `textWrap` defaults to `"maintain"` — neither auto-wraps or auto-unwraps prose on its own. If no tool will maintain a convention automatically, the maintenance burden falls entirely on whoever edits the file by hand, which is a strong argument for matching whatever convention a repo's existing markdown already uses rather than introducing a new, inconsistent one. Absent any such convention already in place, default to the two rules above.
