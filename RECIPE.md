# Auto Tabs

Herdr's tab bar, except that a tab nobody named is labeled with what its lead pane is doing — the live terminal title an agent sets (Claude Code's session title, for example) — instead of its position number. Named tabs keep their names.

## What you see

- **Bar:** identical to the shipped `tabs` mod: a `chrome` strip with a bottom `border`, tabs as 29 px pills (7 px radius, 12.8 px text; active one `raised` with a `border` outline, others `chrome` with `muted` text), the program icon at 14 px, the label truncated at 240 px (170 px in the shipped bar), a `working` spinner, a `blocked`/`done` dot, a close × on the active tab, then `+`, Files and Changes buttons and Windows window buttons.
- **Labels:** a named tab shows its name. An unnamed one shows, in order of preference: the lead pane's terminal title; the program's name for a non-shell program; the basename of the lead pane's folder for a shell; else Herdr's own name (its position).

## How it behaves

- Lead pane of a tab: among its terminal panes (`panes()` with `tab === tab.id` and a `terminal`), the first whose `program.agent` is true, else the focused one, else the first.
- A title counts only if, trimmed, it doesn't start with `~`, `/` or a drive letter path and contains no `@` (shells' `user@host:~/dir` titles), and, after stripping leading non-letter, non-digit characters (spinners like `✳`, `⠋`) and collapsing whitespace, is non-empty and is neither the program's `name` nor its `key`. More than 48 characters: the first 47 plus `…`.
- Labels follow the title live: every change in `panes()` redraws the bar. No timer.
- Nothing calls `tab.rename`. Double-click or F2 renames as before; once named, the name wins; renaming to `""` brings the auto label back.
- Zoomed tabs append ` Z`, as in the shipped bar. Drag, wheel, right-click menu, + menu and panel buttons are unchanged.

## Built from

- A copy of the shipped `tabs` mod (`mod.clone`), with its own SVG paths changed to `mods/auto-tabs/`.
- `mod.json`: `{ "place": "none", "title": "Auto Tabs" }`; the layout's tab bar block names it (`"content": { "mod": "auto-tabs" }`).
- `herdr`: `spaces()` (`tabs[i].named`, `name`, `id`), `panes()` (`tab`, `terminal`, `title`, `cwd`, `focused`, `program.{agent, shell, key, name}`), and the shipped bar's commands (`tab.select`, `tab.move`, `tab.rename`, `tab.close`, `tab.prev`/`tab.next`, `layout.*`).
- Uses the shipped mods' assets `mods/workspace-chrome/spinner.svg`, `mods/pane/close.svg`, `mods/files/file.svg`, `mods/changes/changes.svg`, and opens the shipped `menu` and `new-menu` overlays.

## Watch out

- Unnamed tabs' `name` is their position (`"2"`); use `named`, not a pattern on `name`, to tell them apart.
- Run the path and `@` tests on the raw title: stripping leading symbols first turns `~/Documents/x` into `Documents/x`, which then passes.
- Terminal titles from `hc ls` are not listed; `panes()` and `hc get <t>` carry them.
- Program `key` `"program"` means "not in Herdr's table": treat it like a shell and fall back to the folder.

## Done when

- `herdr-app check mods/auto-tabs` passes.
- With Claude Code working in an unnamed tab, the tab shows the session title and updates when it changes.
- A tab with a focused shell and an agent pane shows the agent's title.
- A shell-only tab whose title is `~/Projects/app` shows `app`.
- Renaming a tab shows the name; renaming it to `""` returns the auto label.
