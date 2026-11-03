# Auto Tab Titles for Herdr

**Tabs named after the work in them, in the [Herdr app](https://herdr.dev).** A tab you never named shows what its agent is doing — `Fix login redirect`, not `1` — read live from the terminal title that Claude Code, Codex, pi and most agents set. A tab you name yourself keeps your name.

It is Herdr's own tab bar with one change, so everything else (drag to reorder, double-click or F2 to rename, the right-click menu, status spinners, Files / Changes buttons) works as before.

## What you get

| In the tab | Tab shows |
|---|---|
| Claude Code working on a task | `Fix login redirect` (its session title, live) |
| An agent that hasn't set a title yet | `Claude Code` |
| Neovim, htop, any program with a title | its title, e.g. `auth.ts (~/app) - NVIM` |
| A program without one | its name, e.g. `Neovim` |
| A shell (title is a path or `user@host`) | its folder, e.g. `dashboard` |
| A tab you renamed | your name, always |

- **Several panes:** the tab follows the agent's pane, else the focused pane, else the first.
- **Nothing is renamed.** The titles are only drawn by the tab bar; the tab stays unnamed in Herdr, on every screen and machine. Rename a tab to pin a name; rename it to empty (`""`) to hand it back.
- Leading spinner and state glyphs (`✳`, `⠋`, `·`) are dropped; titles longer than 48 characters end in `…`.

Herdr itself only shows these titles in the agents list; its tab bar shows `1`, `2`, `3`. And the [herdr-auto-title](https://github.com/kryptamine/herdr-auto-title) plugin is for the herdr terminal app (`herdr server`), not the Herdr desktop app.

## Install

1. Copy `mods/auto-tabs` into `~/.config/herdr-app/mods/`.
2. In your layout (`~/.config/herdr-app/layouts/<name>.json`), point the tab bar block at it: replace `"mod": "tabs"` with `"mod": "auto-tabs"`. Herdr's own layouts are read-only, so clone one first (`mod.clone` / `layout.clone`) if you use `main` as shipped.
3. Run `herdr-app check ~/.config/herdr-app/mods/auto-tabs`.

Or ask the agent in your Herdr terminal: *"add herdr.dev/mods/jenthous/herdr-app-auto-tab-titles"*.

**Already have your own tab bar mod?** The whole feature is the `tabTitle(tab, panes)` function at the top of `mods/auto-tabs/main.js` (about 35 lines, no dependencies). Copy it into your mod and use `tabTitle(c, panes())` where it draws a tab's name.

## Requirements

Herdr app 0.1.12 or later. The mod reads only `spaces()` and `panes()`: no network, no programs, no files outside its folder.

## License

MIT
