# plainhtml reference

The full catalog for building css-less front ends. Follow SKILL.md rules:
no CSS, no classes, no assets, vanilla JS only, literal copy, native elements.

---

## 1. Element catalog

### Structure & boxes

| Element | Renders as | Notes |
|---|---|---|
| `<h1>`…`<h6>` | Bold headings, shrinking | One `h1` per page; `h2`+ inside large panels if a `legend` isn't enough |
| `<p>` | Paragraph with margins | Free vertical rhythm — use empty `<p>`s sparingly, prefer content spacing |
| `<hr>` | Full-width rule | Section divider, also closes a page before the footer |
| `<fieldset>` + `<legend>` | Bordered box, title notched in top border | THE panel. `min-inline-size: min-content` by default: keep content wrappable |
| `<fieldset disabled>` | Grayed box, all controls locked | Great for "engine not loaded" states |
| `<details>` + `<summary>` | Collapsible box with triangle marker | Native accordion. `open` attribute starts expanded |
| `<details name="x">` | Same, but exclusive within `name` | Opening one closes the others — css-less tabs! |
| `<dialog>` | Hidden by default; `showModal()` = centered modal + backdrop | Close via `<form method="dialog">` button |
| `[popover]` + `popovertarget` | Hidden by default; button floats it top-layer | Light-dismiss menus/tooltips; `popovertargetaction="show"` for hover-less popups |
| `<table>` / `<tr>` / `<td valign="top">` | Ruled grid | The ONLY column layout tool. `colspan`/`rowspan` work |
| `<blockquote>` | Indented slab | Alerts, notes, quoted speech, system messages |
| `<pre>` | Monospace, whitespace preserved | Logs, ASCII art, bar charts, diagrams, aligned stats |

### Form controls

| Element | Renders as | Notes |
|---|---|---|
| `<button type="button">` | Chunky native button | Default icon = text/emoji. `<button onclick=...>` for actions |
| `<button type="submit">` | Same, submits form | Use once per form for the primary action, or not at all |
| `<input type="text">` | Inset text box | Size with `size=` (chars). `placeholder` for hints |
| `<input type="search">` | Text box (with clear affordance in some browsers) | Good for filter bars; pair with `list=` + `<datalist>` |
| `<input type="number" min max step>` | Spinner box | Numeric params |
| `<input type="range" min max value>` | Slider | The css-less "knob". Read via `oninput` |
| `<input type="color">` | Color swatch button | Native picker |
| `<input type="date">` / `time` / `datetime-local` | Box with picker icon | Native pickers |
| `<input type="file">` | Browse button + filename | Native file chooser |
| `<input type="checkbox">` | Checkbox | Wrap in `<label>` so text toggles it |
| `<input type="radio" name=...>` | Radio | Exclusive per `name`; wrap each in `<label>` |
| `<select>` | Dropdown | `size=` → list box; `multiple` → multi-select; `<optgroup label=...>` groups |
| `<datalist>` + `list=` | Text box with suggestion dropdown | Combobox, free text allowed |
| `<textarea rows= cols=>` | Inset multi-line box | `rows`/`cols` are the width/height. `placeholder` works |
| `<output>` | Inline status text | Semantically "the result"; also styled mildly like a field |
| `<progress max value>` | Blue determinate bar | Omit `value` for spinner (indeterminate). Needs `style="width:100%"` to fill |
| `<meter min max value low high optimum>` | Green/yellow/red gauge | The only colorful element. Also needs `style="width:100%"` to fill |
| `<fieldset disabled>` | Disables everything inside | Toggle with `fs.disabled = true/false` |
| `<label for>` | Plain text | Always pair with control `id`, or wrap the control |

### Text-level visuals

| Element | Default look | Use for |
|---|---|---|
| `<mark>` | Yellow highlight | Active state, "this is the important bit" |
| `<strong>` / `<em>` | Bold / italic | Emphasis, values |
| `<del>` / `<ins>` | Strike / underline | Removed vs added lines (diffs, changelogs) |
| `<u>` / `<s>` | Underline / strike | Links-looking text if you must; de-emphasis |
| `<small>` | Smaller text | Tile labels, footnotes, secondary metadata |
| `<code>` / `<samp>` / `<var>` / `<kbd>` | Monospace (kbd looks like a keycap) | Commands, output, variables, keyboard keys |
| `<abbr title="...">` | Text with dotted underline | Tooltips on hover — free interactivity |
| `<sub>` / `<sup>` | Raised/lowered small text | Footnote marks, units, exponents |
| `<q>` / `<cite>` | Quoted / italic citation | Inline quotes |
| Emoji + glyphs | As-is | The icon set: ▲▼ ●○ ✓✕ ⚠ ℹ → ─ ▶ ⏹ 🎤 🗑 ⚙ ⏳ |

---

## 2. Attribute allowlist

Presentational / layout attributes that are permitted (they are HTML, not CSS):

- `valign="top"` on `<td>` / `<th>` — top-align columns (default is middle).
- `width` / `height` on `<td>`, `<th>`, `<img>`, `<video>`, `<canvas>`,
  `<iframe>`, `<input type="image">` — column sizing, media sizing.

Structural/functional attributes are always fine: `id`, `name`, `type`,
`value`, `checked`, `selected`, `disabled`, `readonly`, `required`,
`placeholder`, `hidden`, `open`, `rows`, `cols`, `size`, `multiple`, `span`,
`colspan`, `rowspan`, `min`, `max`, `step`, `low`, `high`, `optimum`,
`maxlength`, `pattern`, `list`, `for`, `lang`, `dir`, `title`, `target`,
`href`, `action`, `method`, `popover`, `popovertarget`, `popovertargetaction`,
`draggable`, `contenteditable`, `autofocus`, `autocomplete`, `accept`,
`inputmode`, `start`, `reversed`, `loop`, `controls`, `autoplay`, `muted`,
`datetime`, `cite`, `download`, `hreflang`, `media`, `sizes`, `src`,
`srcset`, `alt`, `loading`, `decoding`.

(`align` and `bgcolor` are NOT allowed here — see section 7 escape hatches.)

`style` is permitted exactly once per element kind: width (or
`width:100%`) on `<progress>` and `<meter>`.

---

## 3. Recipe library

### 3.1 Page shell (control panel / settings app)

```html
<h1>Machine Control</h1>
<p>Load a model, run jobs, watch the gauges. Rendered by your browser, no CSS.</p>

<form id="app" onsubmit="return false;">
  <fieldset>
    <legend>Engine</legend>
    ...
  </fieldset>
  <fieldset>
    <legend>Jobs</legend>
    ...
  </fieldset>
  <hr>
  <p><output id="status">Ready.</output></p>
</form>
```

### 3.2 Toolbar row

```html
<div>
  <button type="button" id="run-btn">▶ Run</button>
  <button type="button" id="stop-btn">⏹ Stop</button>
  <button type="button" id="clear-btn">🗑 Clear</button>
</div>
```

Buttons are inline-block: whitespace in the source becomes the gap. One
`<div>` per visual row. Keep rows to 2–6 buttons; wrap more into a second row.

### 3.3 Label + field pairs

```html
<label for="repo">Repo:</label>
<input id="repo" type="text" size="40" placeholder="owner/model">
<label for="file">File:</label>
<input id="file" type="text" size="40" placeholder="model-q4.gguf">
```

Inline flow: label, field, label, field — dense and readable. For one-field-
per-line, wrap each pair in its own `<p>` or `<div>`.

### 3.4 Stat tile grid

```html
<table>
  <tr>
    <td valign="top" width="25%"><small>Uptime</small><br><strong>14d 6h</strong></td>
    <td valign="top" width="25%"><small>Requests</small><br><strong>1,204</strong></td>
    <td valign="top" width="25%"><small>Error rate</small><br><strong>0.3%</strong></td>
    <td valign="top" width="25%"><small>Queue</small><br><strong>2</strong></td>
  </tr>
</table>
```

### 3.5 Dashboard columns (sidebar + main)

```html
<table>
  <tr>
    <td valign="top" width="30%">
      <fieldset><legend>Navigation</legend> ... </fieldset>
    </td>
    <td valign="top" width="70%">
      <fieldset><legend>Overview</legend> ... </fieldset>
      <fieldset><legend>Details</legend> ... </fieldset>
    </td>
  </tr>
</table>
```

Each cell holds whole panels. Nest `table`s inside cells for finer tiles.

### 3.6 Data table with live state

```html
<table>
  <tr><th>Service</th><th>Status</th><th>Latency</th><th></th></tr>
  <tr>
    <td>api</td>
    <td><mark>✓ healthy</mark></td>
    <td><meter min="0" max="200" low="50" high="120" optimum="20" value="42"></meter> 42ms</td>
    <td><button type="button">Restart</button></td>
  </tr>
  <tr>
    <td>worker</td>
    <td>⚠ degraded</td>
    <td><meter min="0" max="200" low="50" high="120" optimum="20" value="160"></meter> 160ms</td>
    <td><button type="button">Restart</button></td>
  </tr>
</table>
```

### 3.7 Exclusive accordion = tabs

```html
<details name="view" open><summary>Chart</summary> ... </details>
<details name="view"><summary>Table</summary> ... </details>
<details name="view"><summary>Log</summary> ... </details>
```

Opening one closes the others. Zero JS.

### 3.8 Modal dialog

```html
<button type="button" onclick="dlg.showModal()">Open log</button>
<dialog id="dlg">
  <form method="dialog">
    <fieldset>
      <legend>Job log</legend>
      <pre id="log">...</pre>
      <button>Close</button>
    </fieldset>
  </form>
</dialog>
```

The `<form method="dialog">` makes any button inside close the dialog — no JS
needed for closing. Esc closes it too.

### 3.9 Popover menu

```html
<button type="button" popovertarget="menu">Actions ▾</button>
<div id="menu" popover>
  <p><button type="button">Duplicate</button></p>
  <p><button type="button">Export</button></p>
  <p><button type="button">Delete</button></p>
</div>
```

Pops up top-layer, closes on outside click. Each button on its own line reads
better in a popover (they stack as block buttons in `<p>`s).

### 3.10 ASCII bar chart / sparkline (`<pre>`)

```html
<pre>
  cpu  ████████░░░░░░░░  52%
  mem  ████████████░░░░  78%
  disk █████░░░░░░░░░░░  31%
  gpu  ██░░░░░░░░░░░░░░  12%
</pre>
```

Block chars: █ (full) ▓ ▒ ░ (light) · (dot) ─ │ ┌┐└┘ for frames. Build rows
in JS: `"█".repeat(n) + "░".repeat(max - n)`.

### 3.11 Chat / message transcript

```html
<fieldset>
  <legend>Chat</legend>
  <div>
    <button type="button" id="clear-btn">🗑 Clear history</button>
  </div>
  <div id="messages">
    <p><strong>You:</strong> Why is the queue backing up?</p>
    <p><strong>Model:</strong> The worker pool is capped at 2. Raise it or drain slowly.</p>
  </div>
</fieldset>
```

Alternative flavor for long turns: `<blockquote><p><strong>You:</strong>
...</p></blockquote>` — each turn becomes an indented slab.

### 3.12 Wizard / progressive disclosure

Prefer native state:

```html
<details name="wizard" open><summary>1. Source</summary> ... </details>
<details name="wizard"><summary>2. Options</summary> ... </details>
<details name="wizard"><summary>3. Confirm</summary> ... </details>
```

Or reveal sections with `hidden` + buttons:

```html
<button type="button" onclick="step2.hidden = false">Next →</button>
<fieldset id="step2" hidden><legend>Step 2</legend> ... </fieldset>
```

### 3.13 Search / filter bar

```html
<div>
  <label for="q">Filter:</label>
  <input id="q" type="search" size="30" list="known" placeholder="type or pick...">
  <datalist id="known">
    <option value="api"></option>
    <option value="worker"></option>
  </datalist>
  <button type="button" id="apply-btn">Apply</button>
  <button type="button" id="reset-btn">Reset</button>
</div>
```

### 3.14 Disabled-until-ready group

```html
<fieldset id="controls">
  <legend>Inference (load a model first)</legend>
  <textarea id="prompt" rows="3" cols="60"></textarea>
  <div><button type="button" id="send-btn">▶ Send</button></div>
</fieldset>
<script>
  // when the model loads:
  controls.disabled = false;
</script>
```

### 3.15 Inline editable notes

```html
<p contenteditable="true">Click to edit this note...</p>
```

A contenteditable element is a css-less text editor. Read `el.innerText` on
save.

---

## 4. JS micro-patterns

All optional. Keep the whole script under ~100 lines.

```html
<script>
  // IDs are reachable as globals in browsers, but prefer explicit lookup:
  const $ = (id) => document.getElementById(id);

  // Status line
  const say = (msg) => { $('status').textContent = msg; };

  // Show/hide (never style.display)
  const show = (el) => { el.hidden = false; };
  const hide = (el) => { el.hidden = true; };

  // Simulated job with progress
  $('dl-btn').onclick = () => {
    const bar = $('dl-progress');
    bar.hidden = false;
    let v = 0;
    const t = setInterval(() => {
      bar.value = v += 10;
      say('Downloading... ' + v + '%');
      if (v >= 100) {
        clearInterval(t);
        hide(bar);
        say('Done.');
      }
    }, 120);
  };

  // Append a chat turn
  const addTurn = (who, text) => {
    const p = document.createElement('p');
    const b = document.createElement('strong');
    b.textContent = who + ': ';
    p.append(b, text);        // textContent-safe, no innerHTML needed
    $('messages').append(p);
    p.scrollIntoView();
  };

  // Clear a list/panel
  $('clear-btn').onclick = () => { $('messages').replaceChildren(); say('History cleared.'); };

  // Two-way slider -> meter
  $('gain').oninput = (e) => { $('gain-out').value = e.target.value; };

  // Confirm with native dialog (see 3.8) or window.confirm for one-offs
  // Copy text
  $('copy-btn').onclick = () => navigator.clipboard.writeText($('log').innerText);
</script>
```

Rules for the script: event handlers reference existing IDs, one listener
style (`el.onclick = ...`), no modules/builders/state frameworks, no
`innerHTML` with user text (use `textContent`/`append`).

---

## 5. Visual recipes for "making it look designed" without CSS

1. **Rhythm**: title → lede paragraph → panels → `hr` → footer status. Panels
   of similar content are siblings; don't mix scales.
2. **Density**: combine related controls inline (label field label field);
   separate unrelated groups into their own fieldset. A panel with a single
   lonely control looks wrong — merge or add a `legend` + helper text.
3. **Hierarchy**: `legend` = section title (short!). `small` = secondary.
   `strong` = the number/state that matters. `mark` = the one live value.
4. **Color on demand**: you get exactly three colors for free — `meter`
   (green/yellow/red), `mark` (yellow), `progress` (blue). Use meter for any
   "health/level" visual and let it be the page's color.
5. **Motion on demand**: `progress` indeterminate (no `value`) = spinner;
   `<details>` opening = native slide; `dialog` = native fade/scale.
6. **Emojis as status lights**: ✓ (ok) ⚠ (warn) ✕ (fail) ○ (off) ⏳ (waiting)
   — consistent glyph per state across the whole page.

---

## 6. Pitfalls

| Mistake | Fix |
|---|---|
| `<button>` inside `<form>` submits and reloads | `type="button"` on all non-submit buttons; `onsubmit="return false;"` on the form |
| `class="row"` expecting horizontal layout | Nothing happens without CSS. Wrap inline controls in a `<div>`; they flow inline natively |
| `<div class="card">` expecting a box | `div` is invisible. Use `fieldset`+`legend`, `details`, `blockquote`, or `table` |
| `style="display:none"` to hide | Use the `hidden` attribute; toggle `el.hidden` |
| `progress`/`meter` looks tiny | They size to content; allow `style="width:100%"` (the one sanctioned style) |
| Toolbar buttons wrap at random places | Split into explicit `<div>` rows yourself |
| Long unbroken strings blow up a `fieldset` | Keep content wrappable (spaces/hyphens), or wrap it in `<pre>` deliberately |
| Labels don't focus their input | `label for=` must equal the control's `id` |
| Radio group merges with another | Radios are exclusive per `name` — use distinct `name`s |
| Table columns misalign vertically | `valign="top"` on each `td` |
| Forgetting `<fieldset disabled>` for locked states | Whole-group disable is one attribute |
| Button click reloads page | You used `<button>` without `type` inside the form |
| `select` too small for its options | `size="8"` makes a list box; otherwise width follows longest option + `size` hint |
| Nested forms (invalid HTML) | ONE top-level `<form>`. Use `<button type="button">` + JS, or `<form method="dialog">` inside `<dialog>` only |
| i18n attributes on elements | Don't. Literal copy in the user's language |

---

## 7. Escape hatches (last resort, ask first)

- `<center>` (obsolete but universally supported) when a lone title absolutely
  must center.
- `align="center|right"` on `<td>`/`<th>`/`<p>` when `valign`/table structure
  can't do the job.
- `bgcolor` on `<td>` for a highlighted row — prefer `<mark>` content instead.

Prefer solving layout with structure (tables, fieldsets, block/inline flow)
before touching these.
