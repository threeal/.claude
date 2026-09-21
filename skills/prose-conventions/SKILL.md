---
name: prose-conventions
description: How to write and edit prose — integrating a change into existing text so it reads as written fresh, plus markdown formatting (lists over tables, one line per paragraph). TRIGGER - about to write or edit prose or markdown: a .md file, a GitHub issue/PR/commit body, or a code comment.
---

## Integrating a Change Into Existing Prose

When updating existing prose to reflect a change, don't default to finding the most convenient spot and appending there. Check whether that placement reads smoothly against what's already around it — chronology, topic grouping, tone — and when it wouldn't, rework whatever span is actually affected (a sentence, a paragraph, occasionally the whole document) so it reads as if written fresh rather than bolted on.

This is a balancing judgment, not a mandate to rewrite. Weigh how much a rework improves the narration against how heavy it is, and lean toward the smaller change when the awkwardness is minor — an insertion that already reads fine is both the right call and the cheaper one.

## Markdown Formatting

Prefer lists over tables unless the content is genuinely multi-dimensional — a two-column "name → file" mapping is a list.
Why: tables cost column-width upkeep on every edit, realigning dividers and padding, with no reader benefit over a list for either a human or a model. Reach for one only when there's a real comparison across several columns.

Prefer one line per paragraph or list item over hard-wrapping to a column width.
Why: a wrapped paragraph reads identically either way, but a small edit near the start of one cascades into a multi-line diff instead of a one-line diff — pure noise on every future edit. Contexts that benefit from hard wrapping are the minority that don't soft-wrap.

Both defer to a formatter the repo already configures, so check for one first. Prettier's `proseWrap` defaults to `"preserve"` and `dprint-plugin-markdown`'s `textWrap` to `"maintain"` — neither wraps or unwraps prose on its own. When no tool maintains a convention automatically, match whatever the repo's existing markdown already does rather than introducing an inconsistent one.
