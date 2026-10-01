# plainhtml-me

Plain-HTML skills for Claude Code — complex front ends built from simple HTML
elements with **zero CSS**.

## The skill: `plainhtml`

Builds control panels, dashboards, forms, settings pages, monitors, and other
dense UIs using only native HTML: `fieldset` panels, real form controls,
`table` columns, `details` tabs, `meter` gauges, `dialog` modals, `popover`
menus, emoji icons, and `pre` ASCII charts. No stylesheets, no `style=`, no
`class`, no frameworks, no assets — the browser's default rendering is the
design.

```
skills/plainhtml/
├── SKILL.md              # the skill: rules, visual vocabulary, workflow
├── reference.md          # element catalog, recipes, JS snippets, pitfalls
└── examples/
    ├── gallery.html      # visual catalog of every pattern — open in a browser
    ├── control-panel.html# model loader + chat control panel
    └── dashboard.html    # stat tiles, meters, tabs, modal, popover
```

## Install

Project-level (this repo or any project):

```sh
cp -r skills/plainhtml /path/to/project/.claude/skills/
```

User-level (all your projects):

```sh
cp -r skills/plainhtml ~/.claude/skills/
```

Restart Claude Code (or start a new session) and the skill activates when you
ask for a plain-HTML UI, a css-less front end, or a single self-contained
`.html` app screen.

## See it

Open `skills/plainhtml/examples/gallery.html` in any browser — that page is
the visual reference and has no CSS in it.

## License

Apache-2.0 — see [LICENSE](LICENSE).
