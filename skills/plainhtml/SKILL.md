---
name: plainhtml
description: >-
  Build complex front ends (control panels, dashboards, forms, settings,
  monitors, admin UIs) from plain semantic HTML with zero CSS - no
  stylesheets, no style attributes, no classes, no frameworks. Use whenever
  the user asks for a UI, page, form, app screen, or data visual made of
  plain HTML, "css-less", "no CSS", browser-default styling, a single
  self-contained .html file, or brutalist/native-looking pages.
---

# plainhtml — complex front ends with zero CSS

Build dense, capable UIs that look right under the browser's default
stylesheet alone. The visual language IS the HTML element catalog: bordered
`fieldset` panels, native form controls, `table` columns, `details`
accordions, `meter` gauges, emoji icons, `pre` charts. Think "GitHub markup
with no stylesheet" crossed with a hardware control panel.

## Hard rules

1. **No CSS, ever.** No `<style>`, no `<link rel="stylesheet">`, no `style=""`
   attributes, no CSS-in-JS. Sole exception: `style="width:100%"` (or a fixed
   px width) on `<progress>` and `<meter>`, which have no width attribute.
2. **No `class` attributes.** There is no stylesheet to match them, so they
   are noise. Use `id` for JS/DOM hooks, `name` for radio groups and fields.
3. **No assets.** No web fonts, icon packs, images, or inline SVG. Icons are
   emoji (🎤 ⚠ ✓ ⏳ 🗑 ▶ ⏹) and text glyphs (▲ ▼ ● ○ ✕ → ─).
4. **Vanilla only.** One self-contained `.html` file (unless asked otherwise)
   with at most one small inline `<script>` of vanilla JS at the end of
   `<body>`. Prefer zero JS: native behavior first (`details`, `dialog`,
   `popover`, `<form action>`, `output`).
5. **Literal copy.** UI text written inline in the user's language. No i18n
   attributes, no translation keys, no `data-*` scaffolding.
6. **Native elements only.** Never fake a control with `<div>`s — a `div` is
   invisible without CSS. If content needs a visible box use `fieldset`,
   `table`, `details`, `blockquote`, or `pre`.

## Visual vocabulary

| Need | Use | Default look |
|---|---|---|
| Panel / card | `<fieldset>` + `<legend>` | Bordered box, title notched in the border |
| Page section | stacked `<fieldset>`s in one `<form>` | Stacked bordered panels |
| Toolbar | `<div>` of `<button type="button">` | Buttons line up inline with spaces between |
| Columns / grid | `<table>`, `<td valign="top">` | Real columns |
| Stat tile | `<td>` with `<small>` label + `<strong>` value | Tabular cell |
| Data table | `<table>` + `<th>` | Ruled table with header row |
| Accordion / tabs | `<details>` + `<summary>` (shared `name` = exclusive) | Collapsible box; exclusive group acts like tabs |
| Modal | `<dialog>` + `showModal()` | Centered top-layer box with backdrop |
| Menu / popup | `popover` + `popovertarget` | Floats above page, light-dismiss |
| Gauge | `<meter min max value low high optimum>` | Green/yellow/red bar |
| Progress | `<progress max value>` | Blue determinate bar |
| Status line | `<output>` or `<p id="...">` | Plain text line |
| Alert / note | `<blockquote>` (lead with ⚠ or ℹ) | Indented slab |
| Chart / diagram / log | `<pre>` with block chars (█▓▒░) | Monospace ASCII graphic |
| Divider | `<hr>` | Horizontal rule |
| State / emphasis | `<mark>`, `<del>`, `<ins>`, `<strong>`, `<em>`, `<u>` | Yellow highlight, strike, underline |
| Key / command | `<kbd>`, `<code>`, `<samp>` | Monospace keycap-ish text |
| Disabled group | `<fieldset disabled>` | All controls inside grayed and locked |
| Hidden until needed | `hidden` attribute | Removed from layout entirely |

## Layout rules of thumb

- **Boxes**: `fieldset` is the default panel. Nest `fieldset`s for sub-groups;
  each gets exactly one short `legend`.
- **Rows**: inline controls (`button`, `input`, `select`, `textarea`, `label`,
  `output`) flow side by side when wrapped together in a `<div>`. One `<div>`
  = one visual row of 2–6 controls. Give wide controls (`textarea`, long
  `input`) their own line.
- **Widths**: size with HTML, not CSS — `input size=`, `select size=`,
  `textarea rows= cols=`, `<progress style="width:100%">`.
- **Columns**: one `<table>` with a single `<tr>` holding one `<td
  valign="top">` per column, each cell containing a `fieldset`. Tile grids use
  several `<td>`s per row. `width="25%"`-style width attributes on cells are
  allowed.
- **Page shell** (top to bottom): `h1` title → `p` lede → `form` of stacked
  `fieldset` panels → `hr` → `p` footer with `output` status. Wrap the whole
  UI in one `<form id="app" onsubmit="return false;">`.
- **Allowed presentational attributes (only these)**: `valign="top"` and
  `width`/`height` on table cells and media elements. Nothing else.

## Behavior (keep it tiny)

- Guard the form: `<form id="main" onsubmit="return false;">`; every
  non-submit button gets `type="button"`.
- Show/hide: the `hidden` attribute — `el.hidden = true|false`, toggle with
  `onclick="x.hidden = !x.hidden"`. Never `style.display`.
- Status: `status.textContent = "..."`; results into `<output>` or a `div`.
- Progress: `bar.value = 0..100`; unhide while work runs, hide when done.
- Modal: `onclick="dlg.showModal()"`; close with
  `<form method="dialog"><button>Close</button></form>`.
- Dynamic DOM: `insertAdjacentHTML` / `replaceChildren`, optionally fed from a
  `<template>`. Keep JS under ~100 lines; no async ceremony unless the work
  really is async.

## Workflow

1. Inventory the UI as **panels** (which fieldsets?) and **actions** (which
   buttons/inputs per panel?). Complex apps decompose into 5–10 fieldsets in
   one form.
2. Choose the shell: stacked full-width panels (forms/settings) or a
   `<table>` column grid (dashboards/monitors).
3. Emit one self-contained `.html` file: semantic HTML top-to-bottom,
   `<script>` last, an `id` on everything interactive.
4. Before writing, read `examples/gallery.html` from this skill directory and
   copy its patterns rather than inventing new ones.
5. Sanity-check: every button is `type="button"` (or the one submit), every
   `label` pairs `for`/`id`, initially hidden things use `hidden`, there is no
   `class`, and `style=` appears only as width on `progress`/`meter`.

## Examples (open in a browser)

- `examples/gallery.html` — visual catalog of every pattern.
- `examples/control-panel.html` — model loader + chat control panel with
  toolbar, progress, and log.
- `examples/dashboard.html` — stat tiles, service table with meters, exclusive
  `details` tabs, ASCII chart, modal.

See `reference.md` for the full element catalog, recipe library, JS snippets,
and pitfalls.
