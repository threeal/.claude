---
name: web-frontend-facts
description: Facts about how browser app code runs that are easy to assume wrong, such as when scripts run relative to the first paint. TRIGGER - writing or editing a browser app's entry module (e.g. `src/index.tsx`, `src/main.ts`), its HTML entry (e.g. `index.html`), or its global CSS, or planning anything that must be right on the first frame, like a theme, a layout class, or a redirect.
---

## Module Scripts Run After the First Paint Can Happen

A `<script type="module">` is deferred: it runs only after the document is parsed, and the browser may paint before then. Bundlers like Vite load the app entry this way, so nothing the entry module does arrives in time for the first frame, however early in the file it sits — reading a stored theme before `createRoot`, for example, still shows a dark-mode visitor a flash of the light colors.

Anything that must be right on the first frame has to come from one of these:

- CSS, such as a `prefers-color-scheme` media query for a default theme.
- A classic inline `<script>` (no `type="module"`, no `defer` or `async`) in `<head>`, which runs before the body renders — e.g. to read a saved theme choice from `localStorage` and set a class on `<html>`.

When neither is acceptable, say that the requirement can't be met at first paint instead of proposing a change to the entry module.
